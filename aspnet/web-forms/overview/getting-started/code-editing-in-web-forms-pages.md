---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: Modifica del codice Web Forms ASP.NET in Visual Studio 2013 | Documenti Microsoft
author: Erikre
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 79b10df04432490d6338dadb8f7ddd36192beb3e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880752"
---
<a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a>Codice modifica Web Forms ASP.NET in Visual Studio 2013
====================
da [Erik Reitan](https://github.com/Erikre)

In molte pagine Web Form ASP.NET, scrivere codice in Visual Basic, c# o un altro linguaggio. L'editor di codice in Visual Studio consentono di scrivere rapidamente codice senza commettere errori. Inoltre, l'editor fornisce metodi per creare codice riutilizzabile per ridurre la quantità di lavoro, che è necessario eseguire.

Questa procedura dettagliata vengono illustrate diverse funzionalità dell'editor di codice di Visual Studio.

Durante questa procedura dettagliata, si apprenderà come:

- Correggere gli errori di codifica inline.
- Effettuare il refactoring e ridenominazione del codice.
- Rinominare le variabili e oggetti.
- Inserire frammenti di codice.

## <a name="prerequisites"></a>Prerequisiti


Per completare questa procedura dettagliata, è necessario:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) o [Microsoft Visual Studio Express 2013 per Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework viene installato automaticamente. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 e Microsoft Visual Studio Express 2013 per Web verrà essere noto anche come Visual Studio in tutta questa serie di esercitazioni.  
    >   
    > Se si utilizza Visual Studio, questa procedura dettagliata si presuppone che sia selezionato il **lo sviluppo Web** raccolta di impostazioni la prima volta che viene avviato Visual Studio. Per ulteriori informazioni, vedere [come: Seleziona impostazioni ambiente di sviluppo Web](https://msdn.microsoft.com/library/ff521558.aspx).

  Per un'introduzione a Visual Studio e ASP.NET, vedere [la creazione di una pagina Web Form ASP.NET 4.5 basic in Visual Studio 2013](creating-a-basic-web-forms-page.md).   
 

## <a name="creating-a-web-application-project-and-a-page"></a>Creazione di un progetto di applicazione Web e una pagina

<a id="sectionToggle0"></a>

In questa parte della procedura dettagliata, si verrà creato un progetto di applicazione Web e aggiungere una nuova pagina.

### <a name="to-create-a-web-application-project"></a>Per creare un progetto di applicazione Web

1. Aprire Microsoft Visual Studio.
2. Nel menu **File** selezionare **Nuovo progetto**.  
    ![Menu file](code-editing-in-web-forms-pages/_static/image1.png)

    Verrà visualizzata la finestra di dialogo **Nuovo progetto** .
3. Selezionare il **modelli**  - &gt; **Visual c#**  - &gt; **Web** gruppo di modelli a sinistra.
4. Scegliere il **applicazione Web ASP.NET** modello nella colonna centrale.
5. Denominare il progetto ***BasicWebApp*** e fare clic su di **OK** pulsante.   
![Finestra di dialogo Nuovo progetto](code-editing-in-web-forms-pages/_static/image2.png)
6. Selezionare quindi il **Web Form** modello, quindi scegliere il **OK** pulsante per creare il progetto.  
![Finestra di dialogo Nuovo progetto ASP.NET](code-editing-in-web-forms-pages/_static/image3.png)  

    Visual Studio crea un nuovo progetto che include funzionalità predefinite in base al modello Web Form.


## <a name="creating-a-new-aspnet-web-forms-page"></a>Creazione di una nuova pagina ASP.NET Web Form


Quando si crea una nuova applicazione Web Form mediante la **applicazione Web ASP.NET** modello di progetto, Visual Studio aggiunge una pagina ASP.NET (pagina Web Form) denominata *Default.aspx*, nonché come numerosi altri file e le cartelle. È possibile utilizzare il *Default.aspx* pagina come pagina iniziale per l'applicazione Web. Tuttavia, per questa procedura dettagliata, verranno creare e lavorare con una nuova pagina.

### <a name="to-add-a-page-to-the-web-application"></a>Per aggiungere una pagina all'applicazione Web


1. In **Esplora soluzioni**, fare doppio clic sul nome dell'applicazione Web (in questa esercitazione è il nome dell'applicazione **BasicWebSite**), quindi fare clic su **Aggiungi**  - &gt; **Nuovo elemento**.   
Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Selezionare il **Visual c#**  - &gt; **Web** gruppo di modelli a sinistra. Selezionare quindi **Web Form** dal centro elenco e denominarlo *FirstWebPage*.   
    ![Aggiungi finestra di dialogo Nuovo elemento](code-editing-in-web-forms-pages/_static/image4.png)
3. Fare clic su **Aggiungi** per aggiungere la pagina Web Form al progetto.  
 Visual Studio crea la nuova pagina e viene aperto.
4. Quindi, impostare la nuova pagina come pagina di avvio predefinito. In **Esplora**, fare doppio clic su nuova pagina denominata *FirstWebPage* e selezionare **imposta come pagina iniziale**. Alla successiva esecuzione di questa applicazione per testare l'avanzamento, si visualizzerà automaticamente la nuova pagina nel browser.


## <a name="correcting-inline-coding-errors"></a>Correggere eventuali errori nel codice Inline


L'editor di codice in Visual Studio consente di evitare errori quando si scrive codice, e se sono state apportate a un errore, l'editor di codice consente di correggere l'errore. In questa parte della procedura dettagliata, si scriverà una riga di codice che illustrano le funzionalità di correzione di errore nell'editor.

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a>Per correggere gli errori di codifica semplici in Visual Studio


1. In **progettazione** visualizzare, fare doppio clic sulla pagina vuota per creare un gestore per il **carico** evento per la pagina.   
   Si utilizza il gestore dell'evento solo come punto di scrivere codice.
2. All'interno del gestore, digitare la seguente riga che contiene un errore e premere **invio**:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

   Quando si preme **invio**, editor di codice inserito sottolineature ondulate di colore verde e rossi (comunemente chiamare &quot;ondulate&quot; righe) con aree del codice che presentano problemi. Una sottolineatura verde indica un avviso. Una linea rossa ondulata indica un errore che è necessario correggere. 

    Posiziona il puntatore del mouse su `myStr` per visualizzare una descrizione comando indicante sull'avviso. Inoltre, tenere il puntatore del mouse sulla sottolineatura rossa per visualizzare il messaggio di errore.

    La figura seguente mostra il codice con le sottolineature.

    ![Testo di benvenuto in visualizzazione Progettazione](code-editing-in-web-forms-pages/_static/image5.png "testo di benvenuto in visualizzazione Progettazione")  
   È necessario correggere l'errore mediante l'aggiunta di un punto e virgola `;` alla fine della riga. L'avviso semplicemente che informa che non è stato utilizzato il `myStr` ancora variabile.  

    > [!NOTE] 
    > 
    > Visualizzare il codice corrente la formattazione delle impostazioni di Visual Studio selezionando **strumenti**  - &gt; **opzioni**  - &gt; **tipi di carattere e Colori**.


## <a name="refactoring-and-renaming"></a>La ridenominazione e refactoring

Refactoring è una metodologia di software che implica la ristrutturazione di codice per rendere più semplice da comprendere e gestire, mantenendo la funzionalità. Potrebbe essere un semplice esempio di codice in un gestore eventi per ottenere dati da un database. Quando si sviluppa la pagina, si scopre che è necessario accedere ai dati da diversi gestori diversi. Pertanto, effettuare il refactoring del codice della pagina Creazione di un metodo di accesso ai dati nella pagina e inserendo le chiamate al metodo nei gestori di.

L'editor di codice include strumenti che consentono di eseguire diverse operazioni di refactoring. In questa procedura dettagliata si utilizzeranno due tecniche di refactoring: ridenominazione di variabili e l'estrazione di metodi. Altre opzioni di refactoring sono che incapsula i campi, promuovendo le variabili locali ai parametri del metodo e la gestione dei parametri di metodo. La disponibilità di queste opzioni di refactoring dipende dalla posizione nel codice.

### <a name="refactoring-code"></a>Refactoring del codice

Refactoring uno scenario comune consiste nel creare (Estrai) un metodo dal codice all'interno di un altro membro, ad esempio un metodo. Questo riduce le dimensioni del membro originale e il codice estratto riutilizzabili.

In questa parte della procedura dettagliata, verrà scritto un codice semplice e quindi da cui estrarre un metodo. Refactoring è supportato per c#, pertanto si creerà una pagina che utilizza c# come linguaggio di programmazione.

### <a name="to-extract-a-method-in-a-c-page"></a>Per estrarre un metodo in una pagina in c#

1. Passare a **progettazione** visualizzazione.
2. Nel **della casella degli strumenti**, dal **Standard** scheda, trascinare un [pulsante](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) controllo nella pagina.
3. Fare doppio clic sul **pulsante** controllo per creare un gestore per il relativo [fare clic su](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) evento, quindi aggiungere il codice evidenziato di seguito:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

   Il codice crea un **ArrayList** oggetto utilizza un ciclo per caricarla con i valori e quindi utilizza un altro ciclo per visualizzare il contenuto del **ArrayList** oggetto.
4. Premere **CTRL + F5** per eseguire la pagina e quindi fare clic su di **pulsante** per assicurarsi che verrà visualizzato il seguente output:   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. Tornare all'editor di codice e quindi selezionare le righe seguenti nel gestore eventi.   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. Il pulsante destro la selezione, fare clic su **refactoring**, quindi scegliere **Estrai metodo**. 

    Il **Estrai metodo** viene visualizzata la finestra di dialogo.
7. Nel **nuovo nome del metodo** digitare **DisplayArray**, quindi fare clic su **OK**. 

    L'editor di codice crea un nuovo metodo denominato `DisplayArray`e di inserire una chiamata al metodo nel nuovo di **fare clic su** gestore in cui il ciclo è stato originariamente.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. Premere **CTRL + F5** per eseguire nuovamente la pagina, scegliere il **pulsante**.

    La pagina funziona esattamente come in precedenza. Il `DisplayArray` metodo ora può essere chiamata da qualsiasi posizione nella classe della pagina.

## <a name="renaming-variables"></a>Ridenominazione di variabili

Quando si utilizzano le variabili, come gli oggetti, si potrebbe voler rinominarle dopo che sono già fatto riferimento nel codice. Tuttavia, la ridenominazione di variabili e oggetti può causare l'interruzione se si omette di ridenominare uno dei riferimenti del codice. Pertanto, è possibile utilizzare il refactoring per eseguire la ridenominazione.

### <a name="to-use-refactoring-to-rename-a-variable"></a>Per utilizzare il refactoring per rinominare una variabile


1. Nel **fare clic su** gestore eventi, individuare la riga seguente:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. Fare doppio clic sul nome della variabile `alist`, scegliere **refactoring**, quindi scegliere **rinominare**.

    Il **rinominare** viene visualizzata la finestra di dialogo.
3. Nel **nuovo nome** digitare **ArrayList1** e assicurarsi che il **Anteprima modifiche riferimento** è stata selezionata la casella di controllo. Fare quindi clic su **OK**.

    Il **Anteprima modifiche** la finestra di dialogo e verrà visualizzato un albero che contiene tutti i riferimenti alla variabile che si sta rinominando.
4. Fare clic su **applica** per chiudere la **Anteprima modifiche** la finestra di dialogo.

    Le variabili che fanno riferimento specificamente per l'istanza selezionata vengono rinominate. Si noti tuttavia che la variabile `alist` nella riga seguente non viene rinominato.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    La variabile `alist` in questa riga non viene rinominato perché rappresenta lo stesso valore della variabile `alist` che è stato rinominato. La variabile `alist` nel `DisplayArray` dichiarazione è una variabile locale per il metodo. Nell'esempio viene illustrato che l'utilizzo del refactoring per rinominare variabili sia diverso da semplicemente l'esecuzione di un'azione di Trova e Sostituisci nell'editor. Rinomina le variabili e di conoscere la semantica della variabile che può essere utilizzato con il refactoring.


## <a name="inserting-snippets"></a>Inserimento di frammenti di codice

Poiché esistono che gli sviluppatori di Web Form è spesso necessario per eseguire molte attività di codifica, l'editor di codice fornisce una libreria di frammenti di codice o blocchi di codice predefiniti. È possibile inserire questi frammenti di codice in una pagina.

Ogni linguaggio che si utilizza Visual Studio presenta lievi differenze nella modalità di inserimento di frammenti di codice. Per informazioni sull'inserimento di frammenti di codice, vedere [frammenti di codice IntelliSense di Visual Basic](https://msdn.microsoft.com/library/18yz4be4.aspx). Per informazioni sull'inserimento di frammenti di codice in Visual c#, vedere [frammenti di codice Visual c#](https://msdn.microsoft.com/library/z41h7fat.aspx).

## <a name="next-steps"></a>Passaggi successivi

Questa procedura dettagliata è state illustrate le funzionalità di base dell'editor di codice di Visual Studio 2010 per la correzione degli errori nel codice, il refactoring del codice, la ridenominazione di variabili e l'inserimento di frammenti di codice nel codice. Altre funzionalità dell'editor può rendere più semplice e rapido lo sviluppo di applicazioni. Può, ad esempio, essere necessario:

- Altre informazioni sulle funzionalità di IntelliSense, ad esempio la modifica delle opzioni di IntelliSense e la Gestione frammenti di codice per frammenti di codice in linea. Per altre informazioni, vedere [Using IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx) (Uso di IntelliSense).
- Informazioni su come creare frammenti di codice. Per altre informazioni, vedere [creazione e utilizzo dei frammenti di codice IntelliSense](https://msdn.microsoft.com/library/ms165392.aspx)
- Ulteriori informazioni sulle funzionalità specifiche di Visual Basic di frammenti di codice IntelliSense, ad esempio i frammenti di codice di personalizzazione e la risoluzione dei problemi. Per altre informazioni, vedere [frammenti di codice IntelliSense di Visual Basic](https://msdn.microsoft.com/library/18yz4be4.aspx)
- Altre informazioni su c#-funzionalità specifiche di IntelliSense, ad esempio il refactoring e frammenti di codice. Per ulteriori informazioni, vedere [Visual c# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).
