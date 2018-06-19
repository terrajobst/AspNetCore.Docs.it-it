---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
title: Creazione di un AJAX personalizzata controllo Extender controllo Toolkit (VB) | Documenti Microsoft
author: microsoft
description: Estensioni personalizzate consentono di personalizzare ed estendere le funzionalità dei controlli ASP.NET senza dover creare nuove classi.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 18b29834-c991-4e0c-b533-44d358fbfc9c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 06950770bf788fff4a03e9d41fd448ea675a8bce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875136"
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-vb"></a>Creazione di un'estensione di controllo Toolkit controllo AJAX personalizzati (VB)
====================
by [Microsoft](https://github.com/microsoft)

> Estensioni personalizzate consentono di personalizzare ed estendere le funzionalità dei controlli ASP.NET senza dover creare nuove classi.


In questa esercitazione imparare a creare un'estensione di controllo AJAX Control Toolkit personalizzato. Verrà creata una semplice, ma utile, nuova estensione che modifica lo stato di un pulsante da disabilitata ad abilitata quando si digita il testo in una casella di testo. Dopo avere letto questa esercitazione, sarà in grado di estendere il Toolkit di AJAX ASP.NET con la propria estensione di controllo.

È possibile creare estensioni di controllo personalizzato usando Visual Studio o Visual Web Developer (assicurarsi di avere la versione più recente di Visual Web Developer).

## <a name="overview-of-the-disabledbutton-extender"></a>Panoramica dell'estensione DisabledButton

La nuova estensione di controllo è denominato DisabledButton extender. Questa estensione disporrà di tre proprietà seguenti:

- TargetControlID - casella di testo che si estende il controllo.
- TargetButtonIID - pulsante che è abilitato o disabilitato.
- DisabledText - il testo che viene inizialmente visualizzato nel pulsante. Quando si inizia a digitare, il pulsante Visualizza il valore della proprietà testo del pulsante.

Associare l'estensione DisabledButton a un controllo pulsante e casella di testo. Prima di digitare il testo, il pulsante è disabilitato e la casella di testo e un pulsante simile al seguente:


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image1.png)

([Fare clic per visualizzare l'immagine ingrandita](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image3.png))


Dopo avere avviato la digitazione di testo, il pulsante è abilitato e la casella di testo e un pulsante simile al seguente:


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image4.png)

([Fare clic per visualizzare l'immagine ingrandita](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image6.png))


Per creare l'estensione di controllo, è necessario creare tre file seguenti:

- DisabledButtonExtender.vb - questo file è la classe di controllo sul lato server che consentono di impostare le proprietà in fase di progettazione e gestire la creazione dell'estensione. Definisce inoltre le proprietà che possono essere impostate sul extender. Queste proprietà sono accessibili tramite il codice e in fase di progettazione e corrispondano alle proprietà definite nel file DisableButtonBehavior.js.
- DisabledButtonBehavior.js - Questo file è in cui si aggiungeranno tutta la logica di script client.
- DisabledButtonDesigner.vb - questa classe consente a funzionalità in fase di progettazione. Questa classe è necessario se si desidera che il controllo extender per funzionare correttamente con progettazione di Visual Studio e Visual Web Developer.

Quindi, un'estensione di controllo è costituito da un controllo sul lato server, un comportamento del client e una classe della finestra di progettazione sul lato server. Informazioni su come creare tutti e tre i file nelle sezioni seguenti.

## <a name="creating-the-custom-extender-website-and-project"></a>Creare il progetto e il sito Web di estensione personalizzato

Il primo passaggio consiste nel creare un progetto libreria di classi e il sito Web in Visual Studio e Visual Web Developer. Abbiamo ll creare il programma di estensione personalizzato nel progetto libreria di classi e testare l'estensione personalizzata nel sito Web.

Consente di iniziare con il sito Web s. Seguire questi passaggi per creare il sito Web:

1. Selezionare l'opzione di menu **File, nuovo sito Web**.
2. Selezionare il **sito Web ASP.NET** modello.
3. Denominare il nuovo sito Web *Website1*.
4. Fare clic su di **OK** pulsante.

Successivamente, è necessario creare il progetto di libreria di classi che conterrà il codice per l'estensione di controllo:

1. Selezionare l'opzione di menu **nuovo progetto di File, Add,**.
2. Selezionare il **libreria di classi** modello.
3. Denominare la nuova libreria di classi con il nome **CustomExtenders**.
4. Fare clic su di **OK** pulsante.

Dopo aver completato questi passaggi, la finestra Esplora simile nella figura 1.


[![Soluzione con progetto libreria di classe e il sito Web](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image7.png)

**Figura 01**: soluzione con progetto libreria di classe e il sito Web ([fare clic per visualizzare l'immagine ingrandita](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image9.png))


Successivamente, è necessario aggiungere tutti i riferimenti all'assembly necessari al progetto libreria di classi:

1. Fare clic sul progetto CustomExtenders e selezionare l'opzione di menu **Aggiungi riferimento**.
2. Selezionare la scheda .NET.
3. Aggiungere riferimenti agli assembly riportati di seguito:

    1. System.Web.dll
    2. System.Web.Extensions.dll
    3. System.Design.dll
    4. System.Web.Extensions.Design.dll
4. Selezionare la scheda Sfoglia.
5. Aggiungere un riferimento all'assembly AjaxControlToolkit. dll. Questo assembly si trova nella cartella in cui è stato scaricato AJAX Control Toolkit.

È possibile verificare di avere aggiunto tutti i riferimenti a destra il pulsante destro del progetto e scegliendo proprietà facendo clic sulla scheda Riferimenti (vedere la figura 2).


[![Cartella riferimenti con i riferimenti richiesti](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image10.png)

**Figura 02**: cartella riferimenti con i riferimenti richiesti ([fare clic per visualizzare l'immagine ingrandita](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image12.png))


## <a name="creating-the-custom-control-extender"></a>Creazione di estensione del controllo personalizzato

Ora che è disponibile la libreria di classi, è possibile avviare la creazione del controllo extender. Consente di iniziare con una vasta gamma di una classe di controllo di estensione personalizzato (vedere l'elenco 1) s.

**Elenco 1 - MyCustomExtender.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample1.vb)]

Esistono alcune operazioni si nota di classe di estensione del controllo nel listato 1. In primo luogo, si noti che la classe eredita dalla classe di base ExtenderControlBase. Tutti i controlli extender AJAX Control Toolkit derivano da questa classe di base. Ad esempio, la classe di base include la proprietà TargetID che è una proprietà obbligatoria di ogni controllo extender.

Successivamente, si noti che la classe include i seguenti due attributi correlati per lo script client:

- WebResource - fa sì che un file da includere in una risorsa incorporata in un assembly.
- ClientScriptResource - fa sì che una risorsa script da recuperare da un assembly.

L'attributo WebResource viene utilizzato per incorporare il file MyControlBehavior.js JavaScript nell'assembly quando viene compilato l'estensione personalizzata. L'attributo ClientScriptResource viene utilizzato per recuperare lo script MyControlBehavior.js dall'assembly quando viene utilizzato il programma di estensione personalizzato in una pagina web.


Affinché gli attributi di visualizzazione e ClientScriptResource a funzionare, è necessario compilare il file JavaScript come risorsa incorporata. Selezionare il file nella finestra Esplora soluzioni, aprire la finestra delle proprietà e assegnare il valore *risorsa incorporata* per il **azione di compilazione** proprietà.


Si noti che il controllo extender include anche un attributo TargetControlType. Questo attributo viene utilizzato per specificare il tipo di controllo che verrà esteso mediante l'estensione del controllo. Nel caso di listato 1, il controllo extender viene utilizzato per estendere una casella di testo.

Infine, si noti che l'estensione personalizzata include una proprietà denominata MyProperty. La proprietà è contrassegnata con l'attributo ExtenderControlProperty. I metodi GetPropertyValue() e SetPropertyValue() vengono utilizzati per passare il valore della proprietà dell'estensione di controllo sul lato server per il comportamento del client.

Consente di proseguire e implementare il codice per l'estensione DisabledButton s. Il codice per questa estensione è reperibile nel listato 2.

**Il listato 2 - DisabledButtonExtender.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample2.vb)]

L'estensione DisabledButton listato 2 ha due proprietà denominate TargetButtonID e DisabledText. Il IDReferenceProperty applicata alla proprietà TargetButtonID impedisce l'ID di un controllo pulsante diverso da assegnare alla proprietà.

Gli attributi di visualizzazione e ClientScriptResource associano un comportamento sul lato client che si trova in un file denominato DisabledButtonBehavior.js con questa estensione. Viene illustrato questo file JavaScript nella sezione successiva.

## <a name="creating-the-custom-extender-behavior"></a>La creazione del comportamento di estensione personalizzato

Il componente client-side di un'estensione di controllo viene chiamato un comportamento. La logica effettiva per il pulsante di attivazione e disattivazione è contenuta nel comportamento DisabledButton. Il codice JavaScript per il comportamento è incluso nell'elenco di 3.

**Elenco di 3 - DisabledButton.js**

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample3.js)]

Il file di JavaScript nel listato 3 contiene una classe di sul lato client denominata DisabledButtonBehavior. Questa classe, come relativo doppi sul lato server, include due proprietà denominate TargetButtonID e ottenere DisabledText che è possibile accedere utilizzando\_TargetButtonID/set\_TargetButtonID e ottenere\_DisabledText/set\_ DisabledText.

Il metodo Initialize () si associa l'elemento di destinazione per il comportamento di un gestore dell'evento keyup. Ogni volta che si digita una lettera nella casella di testo associata a questo comportamento, esegue il gestore dell'evento keyup. Il gestore dell'evento keyup Abilita o disabilita il pulsante a seconda che la casella di testo associata al comportamento contenga il testo.

Tenere presente che è necessario compilare il file di JavaScript nel listato 3 come risorsa incorporata. Selezionare il file nella finestra Esplora soluzioni, aprire la finestra delle proprietà e assegnare il valore *risorsa incorporata* per il **azione di compilazione** proprietà (vedere la figura 3). Questa opzione è disponibile in Visual Studio e Visual Web Developer.


[![Aggiunta di un file JavaScript come risorsa incorporata](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image13.png)

**Figura 03**: aggiunta di un file JavaScript come risorsa incorporata ([fare clic per visualizzare l'immagine ingrandita](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image15.png))


## <a name="creating-the-custom-extender-designer"></a>Crea la finestra di progettazione di estensione personalizzato

È disponibile una classe ultimo che è necessario creare per completare l'estensione. È necessario creare la classe della finestra di progettazione nel listato 4. Questa classe è necessaria per rendere il programma di estensione si comportano in modo corretto con progettazione di Visual Studio e Visual Web Developer.

**Listing 4 - DisabledButtonDesigner.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample4.vb)]

Associare la finestra di progettazione nel listato 4 con l'estensione DisabledButton con l'attributo della finestra di progettazione. È necessario applicare l'attributo della finestra di progettazione per la classe DisabledButtonExtender simile al seguente:

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample5.vb)]

## <a name="using-the-custom-extender"></a>Tramite il programma di estensione personalizzato

Ora che è terminata la creazione di estensione del controllo DisabledButton, è possibile utilizzarlo nel sito ASP.NET. In primo luogo, è necessario aggiungere l'estensione personalizzata nella casella degli strumenti. Attenersi ai passaggi riportati di seguito.

1. Facendo doppio clic sulla pagina nella finestra Esplora soluzioni, aprire una pagina ASP.NET.
2. Fare clic sulla casella degli strumenti e selezionare l'opzione di menu **Scegli elementi**.
3. Nella finestra di dialogo Scegli elementi della casella degli strumenti, selezionare l'assembly CustomExtenders.dll.
4. Fare clic su di **OK** per chiudere la finestra di dialogo.

Dopo aver completato questi passaggi, il controllo extender DisabledButton dovrebbe essere visualizzato nella casella degli strumenti (vedere la figura 4).


[![DisabledButton nella casella degli strumenti](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image16.png)

**Figura 04**: DisabledButton nella casella degli strumenti ([fare clic per visualizzare l'immagine ingrandita](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image18.png))


Successivamente, è necessario creare una nuova pagina ASP.NET. Attenersi ai passaggi riportati di seguito.

1. Creare una nuova pagina ASP.NET denominata ShowDisabledButton.aspx.
2. Trascinare uno ScriptManager nella pagina.
3. Trascinare un controllo casella di testo della pagina.
4. Trascinare un controllo pulsante di pagina.
5. Nella finestra Proprietà modificare la proprietà ID pulsante sul valore <em>btnSave</em> e la proprietà di testo per il valore *salvare\**.
  

Una pagina è stata creata con un controllo pulsante e casella di testo ASP.NET standard.

Successivamente, è necessario estendere il controllo casella di testo con l'estensione DisabledButton:

1. Selezionare il **Aggiungi estensione** opzione per aprire la finestra di dialogo Creazione guidata attività (vedere Figura 5). Si noti che la finestra di dialogo include l'estensione DisabledButton personalizzato.
2. Selezionare l'estensione DisabledButton e fare clic su di **OK** pulsante.


[![La finestra di dialogo Creazione guidata](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image19.png)

**Figura 05**: finestra di dialogo dell'estensione guidata ([fare clic per visualizzare l'immagine ingrandita](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image21.png))


Infine, è possibile impostare le proprietà dell'oggetto di estensione DisabledButton. È possibile modificare le proprietà dell'oggetto di estensione DisabledButton modificando le proprietà del controllo casella di testo:

1. Selezionare la casella di testo nella finestra di progettazione.
2. Nella finestra Proprietà espandere il nodo di Extender (vedere Figura 6).
3. Assegnare il valore *salvare* alla proprietà DisabledText e il valore *btnSave* alla proprietà TargetButtonID.


[![Impostazione delle proprietà di estensione](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image22.png)

**Figura 06**: impostazione delle proprietà di estensione ([fare clic per visualizzare l'immagine ingrandita](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image24.png))


Quando si esegue la pagina (premendo F5), il controllo Button è inizialmente disabilitato. Non appena si inizia a immettere il testo nella casella di testo, il pulsante di controllo è abilitato (vedere Figura 7).


[![Il programma di estensione DisabledButton in azione](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image25.png)

**Figura 07**: DisabledButton l'estensione nell'azione ([fare clic per visualizzare l'immagine ingrandita](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image27.png))


## <a name="summary"></a>Riepilogo

L'obiettivo di questa esercitazione è illustrare come è possibile estendere AJAX Control Toolkit con controlli extender personalizzato. In questa esercitazione è stato creato un semplice oggetto extender DisabledButton. Questa estensione è implementato mediante la creazione di una classe DisabledButtonExtender, un comportamento DisabledButtonBehavior JavaScript e una classe DisabledButtonDesigner. Un insieme simile di passaggi è seguire quando si crea un'estensione di controllo personalizzato.

> [!div class="step-by-step"]
> [Precedente](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
