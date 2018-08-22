---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
title: Esegue una convalida semplice (VB) | Microsoft Docs
author: StephenWalther
description: Informazioni su come eseguire la convalida in un'applicazione ASP.NET MVC. In questa esercitazione, Stephen Walther introduce per lo stato del modello e l'helper di convalida HTML...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: df6cf4b7-0bb3-4c4e-b17a-bd78a759a6bc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: 1d0bd6917bab61b17d1cafcf0cd9eb1983275dc8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823964"
---
<a name="performing-simple-validation-vb"></a>Esegue una convalida semplice (VB)
====================
da [Stephen Walther](https://github.com/StephenWalther)

> Informazioni su come eseguire la convalida in un'applicazione ASP.NET MVC. In questa esercitazione, Stephen Walther introduce per lo stato del modello e gli helper di convalida HTML.


L'obiettivo di questa esercitazione è illustrare come è possibile eseguire la convalida all'interno di un'applicazione ASP.NET MVC. Ad esempio, informazioni su come impedire a qualcuno di invio di un modulo che non contiene un valore per un campo obbligatorio. Descrive come usare lo stato del modello e gli helper HTML di convalida.

## <a name="understanding-model-state"></a>Informazioni sullo stato del modello

Si usa lo stato del modello - o, più precisamente, dizionario di stato del modello - per rappresentare errori di convalida. L'azione create () nel listato 1, ad esempio, convalida le proprietà di una classe di prodotto prima di aggiungere la classe di prodotto a un database.


Non sto consigliata aggiungere la logica di convalida o di database a un controller. Un controller deve contenere solo la logica correlata al controllo di flusso dell'applicazione. Si sta eseguendo un collegamento a semplificare le operazioni.


**Listato 1 - Controllers\ProductController.vb**

[!code-vb[Main](performing-simple-validation-vb/samples/sample1.vb)]

Nel listato 1, vengono convalidate le proprietà di nome, descrizione e UnitsInStock della classe di prodotto. Se una di queste proprietà ha esito negativo un test di convalida viene aggiunto un errore al dizionario di stato del modello (rappresentato dalla proprietà ModelState della classe Controller).

Se sono presenti errori nello stato del modello la proprietà ModelState restituisce false. In tal caso, viene visualizzata di nuovo il form HTML per la creazione di un nuovo prodotto. In caso contrario, se non sono presenti errori di convalida, il nuovo prodotto viene aggiunto al database.

## <a name="using-the-validation-helpers"></a>Usando gli helper di convalida

Il framework ASP.NET MVC include due helper di convalida: l'helper Html.ValidationMessage() e l'helper Html.ValidationSummary(). Utilizzare questi due helper in una vista per visualizzare i messaggi di errore di convalida.

Gli helper Html.ValidationMessage() e Html.ValidationSummary() vengono usati nelle visualizzazioni Creazione e modifica vengono generate automaticamente per lo scaffolding di ASP.NET MVC. Seguire questi passaggi per generare la visualizzazione di creazione:

1. L'azione create () nel controller di prodotto e scegliere l'opzione di menu **Aggiungi visualizzazione** (vedere la figura 1).
2. Nel **Aggiungi visualizzazione** finestra di dialogo, selezionare la casella di controllo etichettato **creare una visualizzazione fortemente tipizzata** (vedere la figura 2).
3. Dal **visualizzare i dati classe** elenco a discesa selezionare la classe Product.
4. Dal **visualizzare il contenuto** dall'elenco a discesa selezionare Crea.
5. Fare clic sul pulsante **Aggiungi**.


Assicurarsi che si compila l'applicazione prima di aggiungere una visualizzazione. In caso contrario, non sarà più visualizzato l'elenco delle classi nel **visualizzare i dati classe** nell'elenco a discesa.


[![La finestra di dialogo Nuovo progetto](performing-simple-validation-vb/_static/image1.jpg)](performing-simple-validation-vb/_static/image1.png)

**Figura 01**: aggiunta di una vista ([fare clic per visualizzare l'immagine con dimensioni normali](performing-simple-validation-vb/_static/image2.png))


[![La finestra di dialogo Nuovo progetto](performing-simple-validation-vb/_static/image2.jpg)](performing-simple-validation-vb/_static/image3.png)

**Figura 02**: creazione di una visualizzazione fortemente tipizzata ([fare clic per visualizzare l'immagine con dimensioni normali](performing-simple-validation-vb/_static/image4.png))


Dopo aver completato questi passaggi, si ottiene la visualizzazione di creazione nel listato 2.

**Listato 2 - Views\Product\Create.aspx**

[!code-aspx[Main](performing-simple-validation-vb/samples/sample2.aspx)]

Nel listato 2, l'helper Html.ValidationSummary() viene chiamato immediatamente sopra il form HTML. Questo supporto consente di visualizzare un elenco di messaggi di errore di convalida. L'helper Html.ValidationSummary() esegue il rendering gli errori in un elenco puntato.

Viene chiamato l'helper Html.ValidationMessage() accanto a ogni campo di form HTML. Questo supporto consente di visualizzare un messaggio di errore a destra, accanto a un campo del form. Nel caso listato 2, l'helper Html.ValidationMessage() Visualizza un asterisco, quando si verifica un errore.

La pagina nella figura 3 illustra i messaggi di errore viene eseguito il rendering per gli helper di convalida quando il form viene inviato con i campi mancanti e valori non validi.


[![La finestra di dialogo Nuovo progetto](performing-simple-validation-vb/_static/image3.jpg)](performing-simple-validation-vb/_static/image5.png)

**Figura 03**: crea la vista inviato con problemi ([fare clic per visualizzare l'immagine con dimensioni normali](performing-simple-validation-vb/_static/image6.png))


Si noti che l'aspetto del codice HTML di input campi vengono modificati anche quando si verifica un errore di convalida. I renderer di helper Html.TextBox() una *classe = "errore di convalida input"* attributo quando si verifica un errore di convalida associato alla proprietà viene eseguito il rendering dall'helper Html.TextBox().

Esistono tre le classi foglio di stile utilizzate per controllare l'aspetto degli errori di convalida:

- input-errore di convalida - applicato per il &lt;input&gt; sottoposto a rendering dal Html.TextBox() helper tag.
- campo-errore di convalida - applicato per il &lt;span&gt; tag viene eseguito il rendering dall'helper Html.ValidationMessage().
- Riepilogo-errori di convalida - applicato per il &lt;ul&gt; tag viene eseguito il rendering dall'helper Html.ValidationSumamry().

È possibile modificare queste classi di foglio di stile CSS e quindi modificare l'aspetto di errori di convalida, modificando il file CSS che si trova nella cartella del contenuto.

> [!NOTE] 
> 
> La classe HtmlHelper include le proprietà statiche di sola lettura per il recupero della convalida i nomi relativi a CSS classi. Queste proprietà statiche sono denominate ValidationInputCssClassName, ValidationFieldCssClassName e ValidationSummaryCssClassName.


## <a name="prebinding-validation-and-postbinding-validation"></a>Prebinding Postbinding convalida e convalida

Se si invia il form HTML per la creazione di un prodotto e immettere un valore non valido per il campo price e nessun valore per il campo UnitsInStock, ricevere i messaggi di convalida visualizzati nella figura 4. Questi messaggi di errore di convalida da dove provengono?


[![La finestra di dialogo Nuovo progetto](performing-simple-validation-vb/_static/image4.jpg)](performing-simple-validation-vb/_static/image7.png)

**Figura 04**: gli errori di convalida Prebinding ([fare clic per visualizzare l'immagine con dimensioni normali](performing-simple-validation-vb/_static/image8.png))


Esistono effettivamente due tipi di messaggi di errore di convalida - quelli generati prima che i campi di form HTML sono associati a una classe e quelli generati dopo che i campi del modulo sono associati alla classe. In altre parole, esistono prebinding gli errori di convalida e postbinding gli errori di convalida.

L'azione create () esposto dal controller di prodotto nel listato 1 accetta un'istanza della classe di prodotto. La firma del metodo di creazione simile alla seguente:

[!code-vb[Main](performing-simple-validation-vb/samples/sample3.vb)]

I valori dei campi HTML dal modulo di creazione vengono associati alla classe productToCreate da un componente chiamato un raccoglitore di modelli. Il raccoglitore dei modelli predefiniti aggiunge un messaggio di errore per lo stato del modello automaticamente quando Impossibile associare un campo del form a una proprietà di form.

Lo strumento di associazione del modello predefinito non è possibile associare la stringa "apple" alla proprietà prezzo della classe di prodotto. È possibile assegnare una stringa a una proprietà decimale. Pertanto, lo strumento di associazione del modello aggiunge un errore per lo stato del modello.

Il raccoglitore dei modelli predefiniti inoltre non è possibile assegnare il valore Nothing a una proprietà che non accetta il valore Nothing. In particolare, il gestore di associazione del modello non è possibile assegnare il valore Nothing per la proprietà UnitsInStock. Ancora una volta, lo strumento individuerebbe rinuncia e aggiunge un messaggio di errore allo stato del modello.

Se si vuole personalizzare l'aspetto di questi prebinding i messaggi di errore è necessario creare stringhe di risorse per questi messaggi.

## <a name="summary"></a>Riepilogo

L'obiettivo di questa esercitazione è descrivere i meccanismi di base di convalida nel framework di MVC ASP.NET. Si è appreso come usare lo stato del modello e gli helper HTML di convalida. Abbiamo parlato anche la distinzione tra prebinding e postbinding convalida. In altre esercitazioni si parlerà diverse strategie per spostare il codice di convalida del controller e nelle classi di modelli.

> [!div class="step-by-step"]
> [Precedente](displaying-a-table-of-database-data-vb.md)
> [Successivo](validating-with-the-idataerrorinfo-interface-vb.md)
