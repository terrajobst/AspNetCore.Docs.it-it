---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
title: Esegue la convalida semplice (VB) | Documenti Microsoft
author: StephenWalther
description: Informazioni su come eseguire la convalida in un'applicazione MVC ASP.NET. In questa esercitazione, Stephen Walther introduce di stato del modello e l'helper HTML di convalida...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: df6cf4b7-0bb3-4c4e-b17a-bd78a759a6bc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: efb98d87106e332fffb158e5f382d57fea778957
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="performing-simple-validation-vb"></a>Esegue la convalida semplice (VB)
====================
da [Stephen Walther](https://github.com/StephenWalther)

> Informazioni su come eseguire la convalida in un'applicazione MVC ASP.NET. In questa esercitazione, Stephen Walther introduce di stato del modello e gli helper di convalida HTML.


L'obiettivo di questa esercitazione è illustrare come è possibile eseguire la convalida all'interno di un'applicazione MVC ASP.NET. Ad esempio, come impedisce l'invio di un modulo che non contiene un valore per un campo obbligatorio. Informazioni su come utilizzare lo stato del modello e gli helper HTML di convalida.

## <a name="understanding-model-state"></a>Informazioni sullo stato di modello

Utilizzare lo stato del modello - o in modo più accurato, il dizionario di stato del modello - per rappresentare gli errori di convalida. Ad esempio, l'azione del metodo di creazione nel listato 1 convalida le proprietà di una classe di prodotto prima di aggiungere la classe di prodotto in un database.


Sono non è consigliato aggiungere la logica di convalida o di database a un controller. Un controller deve contenere solo la logica correlata al controllo di flusso dell'applicazione. Si sta eseguendo un collegamento per semplificare le operazioni.


**Elenco 1 - Controllers\ProductController.vb**

[!code-vb[Main](performing-simple-validation-vb/samples/sample1.vb)]

Nel listato 1, vengono convalidate le proprietà UnitsInStock, la descrizione e nome della classe di prodotto. Se una di queste proprietà ha esito negativo un test di convalida viene aggiunto un errore al dizionario di stato del modello (rappresentato dalla proprietà della classe Controller ModelState).

Se sono presenti errori di stato del modello la proprietà ModelState.IsValid restituisce false. In tal caso, verrà nuovamente visualizzato il form HTML per la creazione di un nuovo prodotto. In caso contrario, se non sono presenti errori di convalida, il nuovo prodotto viene aggiunto al database.

## <a name="using-the-validation-helpers"></a>Utilizzando l'helper di convalida

Il framework di MVC ASP.NET include due helper di convalida: l'helper Html.ValidationMessage() e il supporto Html.ValidationSummary(). Utilizzare questi due helper in una vista per visualizzare i messaggi di errore di convalida.

Gli helper Html.ValidationMessage() e Html.ValidationSummary() vengono utilizzati nelle viste di creare e modificare che vengono generate automaticamente dallo scaffolding di ASP.NET MVC. Seguire questi passaggi per generare la visualizzazione di creazione:

1. L'azione del metodo di creazione del controller di prodotto e scegliere l'opzione di menu **Aggiungi visualizzazione** (vedere la figura 1).
2. Nel **Aggiungi visualizzazione** finestra di dialogo, selezionare la casella di controllo con etichettata **creare una visualizzazione fortemente tipizzata** (vedere la figura 2).
3. Dal **visualizzare dati classe** elenco a discesa, selezionare la classe di prodotto.
4. Dal **visualizzare il contenuto** crea seleziona nell'elenco a discesa.
5. Fare clic sul pulsante **Aggiungi**.


Assicurarsi che si compila l'applicazione prima di aggiungere una visualizzazione. In caso contrario, l'elenco delle classi non verrà visualizzata nella **visualizzare dati classe** elenco a discesa.


[![La finestra di dialogo Nuovo progetto](performing-simple-validation-vb/_static/image1.jpg)](performing-simple-validation-vb/_static/image1.png)

**Figura 01**: aggiunta di una vista ([fare clic per visualizzare l'immagine ingrandita](performing-simple-validation-vb/_static/image2.png))


[![La finestra di dialogo Nuovo progetto](performing-simple-validation-vb/_static/image2.jpg)](performing-simple-validation-vb/_static/image3.png)

**Figura 02**: creazione di una visualizzazione fortemente tipizzata ([fare clic per visualizzare l'immagine ingrandita](performing-simple-validation-vb/_static/image4.png))


Dopo aver completato questi passaggi, si ottiene la visualizzazione di creazione nel listato 2.

**Il listato 2 - Views\Product\Create.aspx**

[!code-aspx[Main](performing-simple-validation-vb/samples/sample2.aspx)]

Nel listato 2, l'helper Html.ValidationSummary() viene chiamato immediatamente sopra il form HTML. Questo supporto viene utilizzato per visualizzare un elenco di messaggi di errore di convalida. L'helper Html.ValidationSummary() esegue il rendering gli errori in un elenco puntato.

L'helper Html.ValidationMessage() viene chiamato accanto a ognuno dei campi del form HTML. Questo supporto viene utilizzato per visualizzare un messaggio di errore destra accanto a un campo del form. Nel caso di listato 2, l'helper Html.ValidationMessage() viene visualizzato un asterisco quando si verifica un errore.

La pagina nella figura 3 illustra i messaggi di errore visualizzati per gli helper di convalida quando il form viene inviato con i campi mancanti e valori non validi.


[![La finestra di dialogo Nuovo progetto](performing-simple-validation-vb/_static/image3.jpg)](performing-simple-validation-vb/_static/image5.png)

**Figura 03**: crea la vista inviato con i problemi ([fare clic per visualizzare l'immagine ingrandita](performing-simple-validation-vb/_static/image6.png))


Si noti che l'aspetto del codice HTML input campi vengono modificati anche quando si verifica un errore di convalida. I renderer di helper Html.TextBox() un *classe = "errore di convalida input"* attributo quando si verifica un errore di convalida associati alla proprietà di cui è eseguito il rendering dall'helper della Html.TextBox().

Esistono tre le classi foglio di stile utilizzate per controllare l'aspetto di errori di convalida:

- Errore di convalida input - applicato per il &lt;input&gt; tag sottoposto a rendering dal supporto Html.TextBox().
- Errore - convalida campo collegato il &lt;span&gt; tag dopo il rendering dall'helper della Html.ValidationMessage().
- errori di riepilogo convalida - applicato per il &lt;ul&gt; tag dopo il rendering dall'helper della Html.ValidationSumamry().

È possibile modificare queste classi di foglio di stile CSS e quindi modificare l'aspetto di errori di convalida, modificando il file Site.css che si trova nella cartella del contenuto.

> [!NOTE] 
> 
> La classe HtmlHelper include le proprietà statiche di sola lettura per il recupero dei nomi della convalida correlati a CSS classi. Queste proprietà statiche sono denominate ValidationInputCssClassName, ValidationFieldCssClassName e ValidationSummaryCssClassName.


## <a name="prebinding-validation-and-postbinding-validation"></a>Prebinding convalida e convalida Postbinding

Se si invia il form HTML per la creazione di un prodotto e immettere un valore non valido per il campo Prezzo e nessun valore del campo scorte, si otterrà i messaggi di convalida visualizzati nella figura 4. Da dove provengono questi messaggi di errore di convalida?


[![La finestra di dialogo Nuovo progetto](performing-simple-validation-vb/_static/image4.jpg)](performing-simple-validation-vb/_static/image7.png)

**Figura 04**: errori di convalida Prebinding ([fare clic per visualizzare l'immagine ingrandita](performing-simple-validation-vb/_static/image8.png))


Esistono effettivamente due tipi di messaggi di errore - convalida quelli generati prima i campi di form HTML sono associati a una classe e quelli generato dopo che i campi del modulo sono associati alla classe. In altre parole, sono presenti errori di convalida prebinding e postbinding gli errori di convalida.

L'azione del metodo di creazione esposto dal controller di prodotto nel listato 1 accetta un'istanza della classe di prodotto. La firma del metodo di creazione simile al seguente:

[!code-vb[Main](performing-simple-validation-vb/samples/sample3.vb)]

I valori dei campi nel form HTML il modulo Crea associati alla classe productToCreate da un elemento è stato chiamato un raccoglitore di modelli. Lo strumento di associazione predefinito aggiunge un messaggio di errore per lo stato del modello automaticamente quando Impossibile associare un campo modulo a una proprietà del modulo.

Lo strumento di associazione predefinito non è possibile associare la proprietà di prezzo della classe di prodotto la stringa "apple". È possibile assegnare una stringa a una proprietà decimale. Pertanto, lo strumento di associazione aggiunge un errore per lo stato del modello.

Lo strumento di associazione predefinito inoltre non è possibile assegnare il valore Nothing a una proprietà che non accetta il valore Nothing. In particolare, il gestore di associazione del modello non è possibile assegnare il valore Nothing alla proprietà UnitsInStock. In questo caso, il gestore di associazione del modello rinunciare e aggiunge un messaggio di errore allo stato del modello.

Se si desidera personalizzare l'aspetto di questi messaggi di errore prebinding è necessario creare stringhe di risorse per questi messaggi.

## <a name="summary"></a>Riepilogo

L'obiettivo di questa esercitazione è stata per descrivere il funzionamento di base di convalida nel framework di MVC ASP.NET. È stato illustrato come utilizzare lo stato del modello e gli helper HTML di convalida. Abbiamo parlato anche la distinzione tra prebinding e postbinding convalida. In altre esercitazioni si parlerà diverse strategie per spostare il codice di convalida, i controller e nelle classi del modello.

> [!div class="step-by-step"]
> [Precedente](displaying-a-table-of-database-data-vb.md)
> [Successivo](validating-with-the-idataerrorinfo-interface-vb.md)
