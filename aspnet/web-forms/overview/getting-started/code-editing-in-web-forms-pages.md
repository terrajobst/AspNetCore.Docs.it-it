---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: Modifica del codice Web Form ASP.NET in Visual Studio 2013 | Microsoft Docs
author: Erikre
description: ''
ms.author: riande
ms.date: 03/03/2014
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 5bf10a78b106475ffc01b732b21088bad67eb93d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836239"
---
<a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a>Codice di modifica Web Form ASP.NET in Visual Studio 2013
====================
da [Erik Reitan](https://github.com/Erikre)

In numero di pagine Web Form ASP.NET, si scrive codice in Visual Basic, c# o un'altra lingua. L'editor di codice in Visual Studio consentono di scrivere rapidamente codice senza commettere errori. Inoltre, l'editor fornisce informazioni su come creare codice riutilizzabile per contribuire a ridurre la quantità di lavoro da eseguire.

Questa procedura dettagliata illustra diverse funzionalità dell'editor del codice di Visual Studio.

Durante questa procedura dettagliata, si apprenderà come:

- Correggere gli errori di codifica inline.
- Effettuare il refactoring e ridenominazione del codice.
- Rinominare le variabili e oggetti.
- Inserire frammenti di codice.

## <a name="prerequisites"></a>Prerequisiti


Per completare questa procedura dettagliata, è necessario:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) oppure [Microsoft Visual Studio Express 2013 per Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework viene installato automaticamente. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 e Microsoft Visual Studio Express 2013 per Web verrà essere noto anche come Visual Studio in tutta questa serie di esercitazioni.  
    >   
    > Se si usa Visual Studio, questa procedura dettagliata si presume che il **lo sviluppo Web** raccolta di impostazioni la prima volta che è stato avviato Visual Studio. Per altre informazioni, vedere [procedura: selezionare impostazioni ambiente di sviluppo Web](https://msdn.microsoft.com/library/ff521558.aspx).

  Per un'introduzione a Visual Studio e ASP.NET, vedere [creazione di una pagina Web Form ASP.NET 4.5 base in Visual Studio 2013](creating-a-basic-web-forms-page.md).   
 

## <a name="creating-a-web-application-project-and-a-page"></a>Creazione di un progetto di applicazione Web e di una pagina

<a id="sectionToggle0"></a>

In questa parte della procedura dettagliata, si verrà creare un progetto di applicazione Web e aggiungere una nuova pagina a esso.

### <a name="to-create-a-web-application-project"></a>Per creare un progetto di applicazione Web

1. Aprire Microsoft Visual Studio.
2. Nel menu **File** selezionare **Nuovo progetto**.  
    ![Menu file](code-editing-in-web-forms-pages/_static/image1.png)

    Verrà visualizzata la finestra di dialogo **Nuovo progetto** .
3. Selezionare il **modelli**  - &gt; **Visual c#**  - &gt; **Web** gruppo di modelli a sinistra.
4. Scegliere il **applicazione Web ASP.NET** modello nella colonna centrale.
5. Denominare il progetto ***BasicWebApp*** e fare clic sui **OK** pulsante.   
![Finestra di dialogo Nuovo progetto](code-editing-in-web-forms-pages/_static/image2.png)
6. Selezionare quindi il **Web Form** modello, quindi scegliere il **OK** pulsante per creare il progetto.  
![Finestra di dialogo Nuovo progetto ASP.NET](code-editing-in-web-forms-pages/_static/image3.png)  

    Visual Studio crea un nuovo progetto che include funzionalità predefinite in base al modello Web Form.


## <a name="creating-a-new-aspnet-web-forms-page"></a>Creazione di una nuova pagina ASP.NET Web Form


Quando si crea una nuova applicazione Web Form usando la **applicazione Web ASP.NET** modello di progetto, Visual Studio aggiunge una pagina ASP.NET (pagina Web Form) denominata *default. aspx*, nonché di numerosi altri file e le cartelle. È possibile usare la *default. aspx* pagina come pagina iniziale per l'applicazione Web. Tuttavia, per questa procedura dettagliata, crea e lavorare con una nuova pagina.

### <a name="to-add-a-page-to-the-web-application"></a>Per aggiungere una pagina per l'applicazione Web


1. Nella **Esplora soluzioni**, fare clic sul nome dell'applicazione Web (in questa esercitazione è il nome dell'applicazione **BasicWebSite**), quindi fare clic su **Add**  - &gt; **Nuovo elemento**.   
Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Selezionare il **Visual c#**  - &gt; **Web** gruppo di modelli a sinistra. Quindi, selezionare **Web Form** dalla parte centrale elencare e denominarlo *FirstWebPage*.   
    ![Aggiungi finestra di dialogo Nuovo elemento](code-editing-in-web-forms-pages/_static/image4.png)
3. Fare clic su **Add** per aggiungere la pagina Web Form al progetto.  
 Visual Studio crea la nuova pagina e lo apre.
4. Successivamente, impostare la nuova pagina come pagina di avvio predefinito. Nelle **Esplora soluzioni**, fare doppio clic su nuova pagina denominata *FirstWebPage* e selezionare **imposta come pagina iniziale**. Alla successiva esecuzione di questa applicazione per testare lo stato di avanzamento, verrà visualizzato automaticamente la nuova pagina nel browser.


## <a name="correcting-inline-coding-errors"></a>La correzione degli errori del codice Inline


L'editor di codice in Visual Studio consente di evitare errori quando si scrive codice, e se sono state apportate a un errore, l'editor di codice consente di correggere l'errore. In questa parte della procedura dettagliata, si scriverà una riga di codice che illustrano le funzionalità di correzione di errore nell'editor.

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a>Per correggere gli errori di scrittura di codice semplici in Visual Studio


1. Nella **Design** consente di visualizzare, fare doppio clic sulla pagina vuota per creare un gestore per il **carico** eventi per la pagina.   
   Si usa il gestore dell'evento solo come un'unica posizione per scrivere codice.
2. All'interno del gestore, digitare la riga seguente che contiene un errore e premere **invio**:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

   Quando si preme **invio**, l'editor del codice posiziona le sottolineature di colore verde e rosse (comunemente chiamata &quot;ondulata&quot; righe) in aree del codice che presentano problemi. Una linea verde ondulata indica un avviso. Una sottolineatura rossa indica un errore che è necessario correggere. 

    Sposta il puntatore del mouse su `myStr` per visualizzare una descrizione comando che viene spiegato il messaggio di avviso. Inoltre, tenere premuto il puntatore del mouse sulla sottolineatura rossa per visualizzare il messaggio di errore.

    L'immagine seguente mostra il codice con le sottolineature.

    ![Testo di benvenuto nella visualizzazione della struttura](code-editing-in-web-forms-pages/_static/image5.png "testo di benvenuto nella visualizzazione Progettazione")  
   L'errore deve essere corretti mediante l'aggiunta di un punto e virgola `;` alla fine della riga. L'avviso è sufficiente che informa che non è stato utilizzato il `myStr` variabile ancora.  

    > [!NOTE] 
    > 
    > Visualizzare il codice corrente la formattazione delle impostazioni in Visual Studio facendo clic **degli strumenti**  - &gt; **opzioni**  - &gt; **i tipi di carattere e I colori**.


## <a name="refactoring-and-renaming"></a>La ridenominazione e refactoring

Il refactoring è una metodologia di software che comporta la ristrutturazione del codice per renderlo più facile da comprendere e da gestire, pur conservando la relativa funzionalità. Un semplice esempio possibile scrivere codice nel gestore eventi per ottenere dati da un database. Quando si sviluppa la pagina, si scopre che è necessario accedere ai dati da diversi gestori diversi. Pertanto, eseguire il refactoring del codice della pagina Creazione di un metodo di accesso ai dati nella pagina e inserendo le chiamate al metodo nei gestori di.

L'editor di codice include strumenti che consentono di eseguire diverse attività di refactoring. In questa procedura dettagliata, si utilizzeranno le due tecniche di refactoring: ridenominazione di variabili e l'estrazione dei metodi. Altre opzioni di refactoring sono che incapsula i campi, alzare di livello le variabili locali per i parametri del metodo e la gestione di parametri del metodo. La disponibilità di queste opzioni di refactoring dipende dalla posizione nel codice.

### <a name="refactoring-code"></a>Refactoring del codice

Uno scenario comune di refactoring consiste nel creare (extract) un metodo dal codice che si trova all'interno di un altro membro, ad esempio un metodo. Ciò riduce le dimensioni del membro originale e rende il codice estratto riutilizzabili.

In questa parte della procedura dettagliata, si verrà scrivere semplice codice e quindi estrarre un metodo da quest'ultimo. Refactoring è supportato per il linguaggio c#, in modo che si creerà una pagina che usa c# come linguaggio di programmazione.

### <a name="to-extract-a-method-in-a-c-page"></a>Per estrarre un metodo in una pagina di c#

1. Passare a **progettazione** visualizzazione.
2. Nel **della casella degli strumenti**, dal **Standard** scheda, trascinare un' [pulsante](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) controllo nella pagina.
3. Fare doppio clic il **sul pulsante** controllo per creare un gestore per relativo [fare clic su](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) evento e quindi aggiungere il codice evidenziato seguente:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

   Il codice crea un' **ArrayList** utilizza un ciclo per caricarlo con i valori, oggetto e quindi Usa un altro ciclo per visualizzare il contenuto delle **ArrayList** oggetto.
4. Premere **CTRL+F5** per eseguire la pagina e quindi scegliere il **pulsante** per assicurarsi che viene visualizzato l'output seguente:   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. Tornare all'editor del codice e quindi selezionare le righe seguenti nel gestore dell'evento.   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. Fare doppio clic la selezione, fare clic su **refactoring**, quindi scegliere **Estrai metodo**. 

    Il **Estrai metodo** verrà visualizzata la finestra di dialogo.
7. Nel **nuovo nome del metodo** , digitare **DisplayArray**, quindi fare clic su **OK**. 

    L'editor di codice crea un nuovo metodo denominato `DisplayArray`e di inserire una chiamata al metodo di nuovo nella **fare clic su** gestore in cui il ciclo è stato originariamente.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. Premere **CTRL+F5** per eseguire nuovamente la pagina, scegliere il **pulsante**.

    La pagina funziona esattamente come in precedenza. Il `DisplayArray` metodo può essere chiamata da qualsiasi posizione nella classe della pagina.

## <a name="renaming-variables"></a>Ridenominazione di variabili

Quando si lavora con le variabili, nonché gli oggetti, si potrebbe voler rinominare le colonne dopo che sono già fatto riferimento nel codice. Tuttavia, la ridenominazione di variabili e oggetti può causare l'interruzione se non è disponibile la ridenominazione di uno dei riferimenti del codice. Pertanto, è possibile utilizzare il refactoring per eseguire la ridenominazione.

### <a name="to-use-refactoring-to-rename-a-variable"></a>Per utilizzare il refactoring per rinominare una variabile


1. Nel **fare clic su** gestore eventi, individuare la riga seguente:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. Fare clic sul nome della variabile `alist`, scegliere **effettuare il refactoring**, quindi scegliere **rinominare**.

    Il **Rinomina** verrà visualizzata la finestra di dialogo.
3. Nel **nuovo nome** , digitare **ArrayList1** e assicurarsi che il **Anteprima modifiche riferimento** è stata selezionata la casella di controllo. Fare quindi clic su **OK**.

    Il **Anteprima modifiche** nella finestra di dialogo viene visualizzata e viene visualizzato un albero che contiene tutti i riferimenti alla variabile che si sta rinominando.
4. Fare clic su **Apply** per chiudere la **Anteprima modifiche** nella finestra di dialogo.

    Le variabili che fanno riferimento in modo specifico per l'istanza selezionata vengono rinominate. Si noti tuttavia che la variabile `alist` nella riga seguente non viene rinominato.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    La variabile `alist` in questa riga non viene rinominato poiché non rappresenta lo stesso valore della variabile `alist` che sono stati rinominati. La variabile `alist` nella `DisplayArray` dichiarazione è una variabile locale per tale metodo. Ciò viene illustrato che con refactoring per rinominare le variabili è diverso da quello semplicemente eseguire un'azione trova e Sostituisci nell'editor. refactoring di ridenominazione variabili con conoscenza della semantica della variabile che può essere utilizzato con.


## <a name="inserting-snippets"></a>Inserimento di frammenti di codice

Poiché esistono molte attività di codifica che gli sviluppatori Web Form è spesso necessario per l'esecuzione, l'editor di codice fornisce una libreria di frammenti di codice o blocchi di codice predefiniti. È possibile inserire questi frammenti di codice nella pagina.

Ogni lingua da utilizzare in Visual Studio presenta lievi differenze nella modalità di inserimento di frammenti di codice. Per informazioni sull'inserimento di frammenti di codice, vedere [frammenti di codice IntelliSense di Visual Basic](https://msdn.microsoft.com/library/18yz4be4.aspx). Per informazioni sull'inserimento di frammenti di codice in Visual c#, vedere [frammenti di codice Visual c#](https://msdn.microsoft.com/library/z41h7fat.aspx).

## <a name="next-steps"></a>Passaggi successivi

Questa procedura dettagliata è state illustrate le funzionalità di base dell'editor del codice di Visual Studio 2010 per la correzione degli errori nel codice, il refactoring del codice, la ridenominazione di variabili e inserimento di frammenti di codice nel codice. Funzionalità aggiuntive nell'editor può rendere rapido e semplice lo sviluppo di applicazioni. Può, ad esempio, essere necessario:

- Altre informazioni sulle funzionalità di IntelliSense, ad esempio la modifica delle opzioni di IntelliSense e la Gestione frammenti di codice per frammenti di codice in linea. Per altre informazioni, vedere [Using IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx) (Uso di IntelliSense).
- Informazioni su come creare i propri frammenti di codice. Per altre informazioni, vedere [creazione e uso dei frammenti di codice IntelliSense](https://msdn.microsoft.com/library/ms165392.aspx)
- Altre informazioni sulle funzionalità specifiche per Visual Basic IntelliSense di frammenti di codice, ad esempio i frammenti di codice di personalizzazione e la risoluzione dei problemi. Per altre informazioni, vedere [frammenti di codice IntelliSense di Visual Basic](https://msdn.microsoft.com/library/18yz4be4.aspx)
- Altre informazioni su c#-funzionalità specifiche di IntelliSense, ad esempio il refactoring e frammenti di codice. Per altre informazioni, vedere [Visual c# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).
