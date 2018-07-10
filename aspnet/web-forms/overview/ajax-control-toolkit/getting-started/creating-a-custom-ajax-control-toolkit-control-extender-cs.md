---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
title: Creazione di un AJAX personalizzato controllo controllo Extender Toolkit (c#) | Microsoft Docs
author: microsoft
description: Estensioni personalizzate consentono di personalizzare ed estendere le funzionalità dei controlli ASP.NET senza la necessità di creare nuove classi.
ms.author: aspnetcontent
ms.date: 05/12/2009
ms.assetid: 96b56eca-a892-45a4-96b4-67e61178650a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 525975c9e0bddc6ef539b477e9f5db3cfd7ee8c3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814265"
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-c"></a>Creazione di un controllo Toolkit controllo Extender di AJAX personalizzato (c#)
====================
da [Microsoft](https://github.com/microsoft)

> Estensioni personalizzate consentono di personalizzare ed estendere le funzionalità dei controlli ASP.NET senza la necessità di creare nuove classi.


In questa esercitazione descrive come creare un controllo extender di AJAX Control Toolkit personalizzato. Creiamo un semplice, ma utile, nuova estensione che lo stato di un pulsante cambia da disabilitato ad abilitato quando si digita il testo in una casella di testo. Dopo aver letto questa esercitazione, sarà possibile estendere ASP.NET AJAX Toolkit con i propri dispositivi Extender dei controlli.

È possibile creare dispositivi Extender dei controlli personalizzati utilizzando Visual Studio o Visual Web Developer (assicurarsi di avere la versione più recente di Visual Web Developer).

## <a name="overview-of-the-disabledbutton-extender"></a>Panoramica dell'oggetto Extender DisabledButton

Il nuovo controllo extender viene denominato il dispositivo extender DisabledButton. Questo dispositivo extender hanno tre proprietà:

- TargetControlID - casella di testo che si estende il controllo.
- TargetButtonIID - pulsante che è abilitato o disabilitato.
- DisabledText - il testo che viene inizialmente visualizzato nel pulsante. Quando si inizia a digitare, il pulsante Visualizza il valore della proprietà testo del pulsante.

Associare il dispositivo extender DisabledButton a un controllo TextBox e Button. Prima di digitare un testo, il pulsante è disabilitato e la casella di testo e un pulsante simile al seguente:


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image1.png)

([Fare clic per visualizzare l'immagine con dimensioni normali](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))


Dopo avere avviato la digitazione di testo, il pulsante è abilitato e la casella di testo e un pulsante simile al seguente:


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image4.png)

([Fare clic per visualizzare l'immagine con dimensioni normali](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))


Per creare l'estensione del controllo, è necessario creare i tre file seguenti:

- DisabledButtonExtender.cs - questo file è la classe di controllo lato server che gestisce la creazione dell'extender e consentono di impostare le proprietà in fase di progettazione. Definisce anche le proprietà che possono essere impostate sul dispositivo extender. Queste proprietà sono accessibili tramite il codice e in fase di progettazione e corrispondano alle proprietà definite nel file DisableButtonBehavior.js.
- DisabledButtonBehavior.js: Questo file è dove si aggiungeranno tutta la logica di script client.
- DisabledButtonDesigner.cs - questa classe abilita la funzionalità in fase di progettazione. Questa classe è necessario se si desidera che il controllo extender per funzionare correttamente con progettazione di Visual Studio e Visual Web Developer.

Pertanto, un controllo extender è costituito da un controllo lato server, un comportamento del lato client e una classe della finestra di progettazione sul lato server. Informazioni su come creare tutti e tre i file nelle sezioni seguenti.

## <a name="creating-the-custom-extender-website-and-project"></a>Creare il progetto e il sito Web di estensione personalizzato

Il primo passaggio consiste nel creare un progetto libreria di classi e il sito Web in Visual Studio e Visual Web Developer. Abbiamo ll creare il dispositivo extender personalizzato nel progetto libreria di classi e testare il dispositivo extender personalizzato nel sito Web.

Let s iniziano con il sito Web. Seguire questi passaggi per creare il sito Web:

1. Selezionare l'opzione di menu **File, nuovo sito Web**.
2. Selezionare il **sito Web ASP.NET** modello.
3. Denominare il nuovo sito Web *Website1*.
4. Scegliere il **OK** pulsante.

Successivamente, è necessario creare il progetto di libreria di classi che conterrà il codice per il controllo extender:

1. Selezionare l'opzione di menu **nuovo progetto di File, Add,**.
2. Selezionare il **libreria di classi** modello.
3. Denominare la nuova libreria di classi con il nome **CustomExtenders**.
4. Scegliere il **OK** pulsante.

Dopo aver completato questi passaggi, la finestra Esplora soluzioni dovrebbe essere simile nella figura 1.


[![Soluzione con progetto di libreria di classi e il sito Web](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)

**Figura 01**: soluzione con progetto di libreria di classi e il sito Web ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))


Successivamente, è necessario aggiungere tutti i riferimenti all'assembly necessari per il progetto di libreria di classi:

1. Fare clic sul progetto CustomExtenders e selezionare l'opzione di menu **Aggiungi riferimento**.
2. Selezionare la scheda .NET.
3. Aggiungere riferimenti agli assembly riportati di seguito:

    1. System.Web.dll
    2. System.Web.Extensions.dll
    3. System.Design.dll
    4. System.Web.Extensions.Design.dll
4. Selezionare la scheda Sfoglia.
5. Aggiungere un riferimento all'assembly AjaxControlToolkit. dll. Questo assembly si trova nella cartella in cui è stato scaricato AJAX Control Toolkit.

Dopo aver completato questi passaggi, la cartella riferimenti di progetto libreria di classi dovrebbe essere simile nella figura 2.


[![Cartella riferimenti con i riferimenti necessari](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)

**Figura 02**: cartella riferimenti con i riferimenti necessari ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))


## <a name="creating-the-custom-control-extender"></a>Creazione di estensione del controllo personalizzato

Ora che abbiamo la nostra libreria di classi, è possibile iniziare a creare il controllo extender. Let s iniziano con la vasta gamma di una classe del controllo extender personalizzato (vedere l'elenco 1).

**Listato 1 - MyCustomExtender.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample1.cs)]

Esistono diversi aspetti che noterete sulla classe del controllo extender nel listato 1. In primo luogo, si noti che la classe eredita dalla classe di base ExtenderControlBase. Tutti i controlli extender di AJAX Control Toolkit derivano da questa classe di base. Ad esempio, la classe di base include la proprietà ID destinazione che è una proprietà obbligatoria di ogni controllo extender.

Successivamente, si noti che la classe include i seguenti due attributi correlati per lo script client:

- WebResource - fa sì che un file da includere come risorsa incorporata in un assembly.
- ClientScriptResource - fa sì che una risorsa di script da recuperare da un assembly.

L'attributo WebResource viene utilizzato per incorporare il file MyControlBehavior.js JavaScript nell'assembly quando il dispositivo extender personalizzato viene compilato. L'attributo ClientScriptResource viene utilizzato per recuperare lo script MyControlBehavior.js dall'assembly quando il dispositivo extender personalizzato viene utilizzato in una pagina web.


Affinché gli attributi WebResource e ClientScriptResource funzionamento, è necessario compilare il file JavaScript come risorsa incorporata. Selezionare il file nella finestra Esplora soluzioni, aprire la finestra delle proprietà e assegnare il valore *risorsa incorporata* per il **azione di compilazione** proprietà.


Si noti che il controllo extender include anche un attributo TargetControlType. Questo attributo viene utilizzato per specificare il tipo di controllo che verrà esteso mediante l'estensione del controllo. Nel caso listato 1, il controllo extender viene utilizzato per estendere una casella di testo.

Infine, si noti che il dispositivo extender personalizzato include una proprietà denominata MyProperty. La proprietà è contrassegnata con l'attributo ExtenderControlProperty. I metodi GetPropertyValue() e SetPropertyValue() vengono utilizzati per passare il valore della proprietà dall'estensione del controllo lato server per il comportamento lato client.

Consentire s andare avanti e implementare il codice per il dispositivo extender DisabledButton. Il codice per questa estensione è reperibile nel listato 2.

**Listato 2 - DisabledButtonExtender.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample2.cs)]

Il dispositivo extender DisabledButton nel listato 2 ha due proprietà denominate TargetButtonID e DisabledText. Il IDReferenceProperty applicato alla proprietà TargetButtonID impedisce solo per l'ID di un controllo pulsante assegnando a questa proprietà.

Gli attributi WebResource e ClientScriptResource associare un comportamento del lato client che si trova in un file denominato DisabledButtonBehavior.js con il dispositivo extender. Viene illustrato questo file JavaScript nella sezione successiva.

## <a name="creating-the-custom-extender-behavior"></a>La creazione del comportamento di controllo Extender personalizzato

Il componente lato client di un controllo extender viene chiamato un comportamento. La logica effettiva per la disattivazione e il pulsante è contenuta nel comportamento DisabledButton. Il codice JavaScript per il comportamento è incluso nel listato 3.

**Listato 3 - DisabledButton.js**

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample3.js)]

Il file JavaScript nel listato 3 contiene una classe lato client denominata DisabledButtonBehavior. Questa classe, ad esempio il gemello sul lato server, include due proprietà denominate TargetButtonID e ottenere DisabledText che è possibile accedere usando\_TargetButtonID/set\_TargetButtonID e Introduzione\_DisabledText/set\_ DisabledText.

Il metodo Initialize () si associa l'elemento di destinazione per il comportamento di un gestore dell'evento keyup. Ogni volta che si digita una lettera nella casella di testo associato a questo comportamento, esegue il gestore dell'evento keyup. Il gestore dell'evento keyup Abilita o disabilita il pulsante a seconda che la casella di testo associata al comportamento contenga qualsiasi testo.

Tenere presente che è necessario compilare il file JavaScript nel listato 3 come risorsa incorporata. Selezionare il file nella finestra Esplora soluzioni, aprire la finestra delle proprietà e assegnare il valore *risorsa incorporata* per il **azione di compilazione** proprietà (vedere la figura 3). Questa opzione è disponibile in Visual Studio e Visual Web Developer.


[![Aggiunta di un file JavaScript come risorsa incorporata](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)

**Figura 03**: aggiunta di un file JavaScript come risorsa incorporata ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))


## <a name="creating-the-custom-extender-designer"></a>Creazione della finestra di progettazione di estensione personalizzato

C'è una classe ultimo che è necessario creare per completare l'oggetto extender. È necessario creare la classe della finestra di progettazione contenuta nel listato 4. Questa classe è necessario per rendere il dispositivo extender si comportano in modo corretto con progettazione di Visual Studio e Visual Web Developer.

**Listato 4 - DisabledButtonDesigner.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample4.cs)]

La finestra di progettazione contenuta nel listato 4 associare il dispositivo extender DisabledButton con l'attributo Designer. È necessario applicare l'attributo di progettazione per la classe DisabledButtonExtender simile al seguente:

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample5.cs)]

## <a name="using-the-custom-extender"></a>Utilizza l'Extender personalizzato

Ora che è stata terminata la creazione il controllo extender DisabledButton, è possibile usarlo nel nostro sito Web ASP.NET. In primo luogo, è necessario aggiungere il dispositivo extender personalizzato nella casella degli strumenti. Attenersi ai passaggi riportati di seguito.

1. Facendo doppio clic su pagina nella finestra Esplora soluzioni, aprire una pagina ASP.NET.
2. Fare doppio clic su casella degli strumenti e selezionare l'opzione di menu **Scegli elementi**.
3. Nella finestra di dialogo Scegli elementi della casella degli strumenti, selezionare l'assembly CustomExtenders.dll.
4. Scegliere il **OK** per chiudere la finestra di dialogo.

Dopo aver completato questi passaggi, il controllo extender DisabledButton deve essere visualizzato nella casella degli strumenti (vedere la figura 4).


[![DisabledButton della casella degli strumenti](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)

**Figura 04**: DisabledButton della casella degli strumenti ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))


Successivamente, è necessario creare una nuova pagina ASP.NET. Attenersi ai passaggi riportati di seguito.

1. Creare una nuova pagina ASP.NET denominata ShowDisabledButton.aspx.
2. Trascinare un controllo ScriptManager nella pagina.
3. Trascinare un controllo TextBox nella pagina.
4. Trascinare un controllo pulsante della pagina.
5. Nella finestra Proprietà modificare la proprietà ID del pulsante sul valore <em>btnSave</em> e la proprietà Text sul valore *salvare\**.
  

Viene creata una pagina con un controllo ASP.NET TextBox e Button standard.

Successivamente, è necessario estendere il controllo casella di testo con il dispositivo extender DisabledButton:

1. Selezionare il **Aggiungi estensione** opzione per aprire la finestra di dialogo Creazione guidata attività (vedere la figura 5). Si noti che la finestra di dialogo include l'estensione DisabledButton personalizzato.
2. Selezionare il dispositivo extender DisabledButton e scegliere il **OK** pulsante.


[![La finestra di dialogo Creazione guidata dispositivo Extender](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)

**Figura 05**: finestra di dialogo dell'estensione guidata ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))


Infine, è possibile impostare le proprietà dell'oggetto extender DisabledButton. È possibile modificare le proprietà dell'oggetto extender DisabledButton modificando le proprietà del controllo casella di testo:

1. Selezionare la casella di testo nella finestra di progettazione.
2. Nella finestra Proprietà espandere il nodo dispositivi Extender (vedere la figura 6).
3. Assegnare il valore *salvare* alla proprietà DisabledText e il valore *btnSave* alla proprietà TargetButtonID.


[![Impostazione delle proprietà di estensione](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)

**Figura 06**: l'impostazione delle proprietà di estensione ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))


Quando si esegue la pagina (premendo F5), il controllo Button è inizialmente disabilitato. Non appena si inizia a immettere il testo nella casella di testo, il pulsante di controllo è abilitato (vedere la figura 7).


[![Il dispositivo extender DisabledButton in azione](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)

**Figura 07**: DisabledButton il dispositivo extender in azione ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))


## <a name="summary"></a>Riepilogo

L'obiettivo di questa esercitazione è stata per illustrare come è possibile estendere AJAX Control Toolkit con controlli extender personalizzato. In questa esercitazione è stato creato un semplice DisabledButton controllo extender. Questa estensione è stata implementata tramite la creazione di una classe DisabledButtonExtender, un comportamento DisabledButtonBehavior JavaScript e una classe DisabledButtonDesigner. È seguire una serie di passaggi simile ogni volta che si crea un oggetto extender personalizzato.

> [!div class="step-by-step"]
> [Precedente](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
> [Successivo](get-started-with-the-ajax-control-toolkit-vb.md)
