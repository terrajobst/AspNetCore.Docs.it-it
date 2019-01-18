---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Visualizzare i dati degli elementi e i dettagli | Microsoft Docs
author: Erikre
description: Questa serie di esercitazioni illustra le nozioni di base della creazione di un'applicazione Web Form ASP.NET con ASP.NET 4.7 e Microsoft Visual Studio 2017
ms.author: riande
ms.date: 1/04/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: acc2f8e78375ef0455d467e2af750ecbee623224
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396225"
---
<a name="display-data-items-and-details"></a>Elementi di dati visualizzati e dettagli
====================
da [Erik Reitan](https://github.com/Erikre)

> Questa serie di esercitazioni illustra le nozioni di base della creazione di un'applicazione Web Form ASP.NET con ASP.NET 4.7 e Microsoft Visual Studio 2017.

In questa esercitazione si apprenderà come visualizzare gli elementi di dati e i dettagli dell'elemento dati con Entity Framework Code First e Web Form ASP.NET. Questa esercitazione si basa sull'esercitazione precedente "Navigazione dell'interfaccia utente e" come parte della serie di esercitazioni di Wingtip giocattoli Store. Dopo aver completato questa esercitazione, si noterà i prodotti nel *ProductsList.aspx* pagina e i dettagli di un prodotto nel *ProductDetails.aspx* pagina.

## <a name="youll-learn-how-to"></a>Si imparerà a eseguire le operazioni seguenti:

- Aggiungere un controllo dati per visualizzare i prodotti dal database
- Connettere un controllo dei dati per i dati selezionati
- Aggiungere un controllo dati per visualizzare i dettagli sul prodotto dal database
- Recuperare un valore dalla stringa di query e usare tale valore per limitare i dati recuperati dal database

### <a name="features-introduced-in-this-tutorial"></a>Funzionalità introdotte in questa esercitazione:

- Associazione di modelli
- Provider di valori

## <a name="add-a-data-control"></a>Aggiungere un controllo dei dati

È possibile usare alcune opzioni diverse per associare dati a un controllo server. I più comuni includono:

 * Aggiunta di un controllo origine dati
 * Aggiunta manuale di codice
 * Tramite l'associazione di modelli

### <a name="use-a-data-source-control-to-bind-data"></a>Usare un controllo origine dati per associare i dati

Aggiunta di un controllo origine dati consente di collegare il controllo origine dati al controllo che visualizza i dati. Con questo approccio, è possibile in modo dichiarativo, anziché connettersi a livello di codice i controlli lato server alle origini dati.

### <a name="code-by-hand-to-bind-data"></a>Codice manualmente per associare i dati

Codifica manuale prevede:

1. Lettura di un valore
2. Controllare se è null
3. Conversione in un tipo appropriato
4. Controllo operazioni riuscite di conversione
5. Utilizzando il valore nella query 

Questo approccio consente di avere il controllo completo per la logica di accesso ai dati.

### <a name="use-model-binding-to-bind-data"></a>Usare l'associazione di modelli per associare i dati

Associazione di modelli consente di associare i risultati con molto meno codice e ti offre la possibilità di riusare le funzionalità in tutta l'applicazione. Semplifica l'utilizzo con la logica di accesso ai dati incentrato sul codice garantendo comunque un framework completo e associazione dati.

## <a name="display-products"></a>Visualizzare i prodotti

In questa esercitazione si userà l'associazione di modelli per associare i dati. Per configurare un controllo dati per utilizzare l'associazione di modelli per selezionare i dati, si imposta il controllo `SelectMethod` proprietà a un nome di metodo nel codice della pagina. Il controllo dei dati chiama il metodo momento adeguato del ciclo di vita della pagina e associa automaticamente i dati restituiti. Non è necessario chiamare in modo esplicito il `DataBind` (metodo).

1. Nelle **Esplora soluzioni**aprire *ProductList. aspx*.
2. Sostituire il markup esistente con questo codice:   

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

Questo codice Usa un **ListView** controllo denominato `productList` per visualizzare i prodotti.

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

Con stili e modelli, è possibile definire come il **ListView** controllo Visualizza i dati. È utile per i dati in una struttura ripetuta. Anche se ciò **ListView** viene visualizzata semplicemente i dati del database, è possibile anche, senza codice, consentire agli utenti di modificare, inserire ed eliminare dati e di ordinamento e paging dei dati.

Impostando il `ItemType` proprietà nel **ListView** controllare, l'espressione di associazione dati `Item` è disponibile e il controllo diventa fortemente tipizzato. Come indicato nell'esercitazione precedente, è possibile selezionare i dettagli dell'oggetto elemento con la tecnologia IntelliSense, ad esempio specificando la `ProductName`:

![Visualizzare i dati gli elementi e i dettagli - IntelliSense](display_data_items_and_details/_static/image1.png)

Si usa anche l'associazione di modelli per specificare un `SelectMethod` valore. Questo valore (`GetProducts`) corrisponde al metodo si aggiungeranno al codice indietro per visualizzare i prodotti nel passaggio successivo.

### <a name="add-code-to-display-products"></a>Aggiungere codice per visualizzare i prodotti

In questo passaggio si aggiungerà codice per popolare la **ListView** controllo con i dati prodotti dal database. Il codice supporta che mostra tutti i prodotti e singola categoria.

1. Nelle **Esplora soluzioni**, fare doppio clic su *ProductList. aspx* e quindi selezionare **Visualizza codice**.
2. Sostituire il codice esistente nel *ProductList.aspx.cs* file con questo:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

Questo codice viene illustrato il `GetProducts` metodo che il **ListView** del controllo `ItemType` riferimenti alla proprietà nella *ProductList. aspx* pagina. Per limitare i risultati a una categoria di database specifico, il codice imposta la `categoryId` valore del valore di stringa di query passato al *ProductList. aspx* pagina quando la *ProductList. aspx* pagina è ci si sposta su. Il `QueryStringAttribute` classe la `System.Web.ModelBinding` dello spazio dei nomi viene usato per recuperare il valore della variabile di stringa della query `id`. Ciò indica l'associazione di modelli per tentare di associare un valore di stringa di query di `categoryId` parametro in fase di esecuzione.

Quando una categoria valida viene passata come stringa di query alla pagina, i risultati della query sono limitati a tali prodotti nel database che corrispondono al `categoryId` valore. Ad esempio, se il *ProductsList.aspx* URL della pagina è questo:


[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

Nella pagina vengono visualizzati solo i prodotti in cui il `categoryId` è uguale a `1`.

Tutti i prodotti vengono visualizzati se nessuna stringa di query viene incluso durante la *ProductList. aspx* pagina viene definita.

Le origini dei valori per questi metodi sono dette *valore provider* (ad esempio *QueryString*), e gli attributi di parametro che indicano quali provider di valori da utilizzare sono dette *attributi di provider di valore* (ad esempio `id`). ASP.NET include i provider di valori e gli attributi corrispondenti per tutte le origini tipiche dell'input utente in un'applicazione Web Form, ad esempio la stringa di query, cookie, i valori del form, controlli, lo stato di visualizzazione, lo stato della sessione e le proprietà del profilo. È anche possibile scrivere provider di valori personalizzati.

### <a name="run-the-application"></a>Esecuzione dell'applicazione

Eseguire ora l'applicazione per visualizzare tutti i prodotti o i prodotti della categoria.

1. Premere **F5** all'interno di Visual Studio per eseguire l'applicazione.  
   Il browser si apre e Mostra le *default. aspx* pagina.

2. Selezionare **automobili** dal menu di navigazione di categoria prodotto.  
   Il *ProductList. aspx* pagina viene visualizzato che mostra solo **automobili** prodotti di categoria. Più avanti in questa esercitazione, verranno visualizzati i dettagli del prodotto.  

    ![Visualizzare i dati gli elementi e i dettagli - automobili](display_data_items_and_details/_static/image2.png)

3. Selezionare **prodotti** dal menu di navigazione nella parte superiore.  
   Anche in questo caso, il *ProductList. aspx* pagina viene visualizzata, ma questa volta Mostra l'elenco completo di prodotti.   

    ![Visualizzare i dati gli elementi e i dettagli - prodotti](display_data_items_and_details/_static/image3.png)

4. Chiudere il browser e tornare a Visual Studio.

### <a name="add-a-data-control-to-display-product-details"></a>Aggiungere un controllo dei dati per visualizzare i dettagli sul prodotto

Successivamente, si modificherà il markup nel *ProductDetails.aspx* pagina che è stato aggiunto nell'esercitazione precedente per visualizzare le informazioni di prodotto specifico.

1. Nelle **Esplora soluzioni**aprire *ProductDetails.aspx*.

2. Sostituire il markup esistente con questo codice:

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

    Questo codice Usa un **FormView** controllo per visualizzare i dettagli sul prodotto specifico. Questo markup Usa i metodi, ad esempio i metodi utilizzati per visualizzare i dati nella *ProductList. aspx* pagina. Il **FormView** controllo viene usato per visualizzare un singolo record alla volta da un'origine dati. Quando si usa la **FormView** (controllo), è creare modelli per visualizzare e modificare i valori associati a dati. Questi modelli contengono controlli, espressioni di associazione, e formattazione che definiscono l'aspetto e funzionalità del modulo.

Il markup precedente la connessione al database richiede codice aggiuntivo.

1. Nelle **Esplora soluzioni**, fare doppio clic su *ProductDetails.aspx* e quindi fare clic su **Visualizza codice**.  
   Il *ProductDetails.aspx.cs* file viene visualizzato.

2. Sostituire il codice esistente con il seguente codice:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

Questo codice cerca un "`productID`" valore di stringa di query. Se viene trovato un valore di stringa di query valida, viene visualizzato il prodotto corrisponda. Se non viene trovata la stringa di query o il relativo valore non è valido, non viene visualizzato alcun prodotto.

### <a name="run-the-application"></a>Esecuzione dell'applicazione

A questo punto è possibile eseguire l'applicazione per visualizzare un singolo prodotto visualizzato basato su ID prodotto.

1. Premere **F5** all'interno di Visual Studio per eseguire l'applicazione.  
   Il browser si apre e Mostra le *default. aspx* pagina.

2. Selezionare **evolveranno** dal menu di navigazione di categoria.  
   Il *ProductList. aspx* viene visualizzata la pagina.

3. Selezionare **barca Paper** dall'elenco dei prodotti.
   Il *ProductDetails.aspx* viene visualizzata la pagina.

    ![Visualizzare i dati gli elementi e i dettagli - prodotti](display_data_items_and_details/_static/image4.png)
    
4. Chiudere il browser.


## <a name="additional-resources"></a>Risorse aggiuntive

[Il recupero e visualizzazione dei dati con l'associazione di modelli e web form](../../presenting-and-managing-data/model-binding/retrieving-data.md)

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione è stato aggiunto di markup e codice per visualizzare i prodotti e i dettagli del prodotto. Sono stati illustrati i controlli dati fortemente tipizzato, l'associazione di modelli e i provider di valori. Nella prossima esercitazione, si aggiungerà un carrello della spesa per l'applicazione di esempio Wingtip Toys. 

> [!div class="step-by-step"]
> [Precedente](ui_and_navigation.md)
> [Successivo](shopping-cart.md)
