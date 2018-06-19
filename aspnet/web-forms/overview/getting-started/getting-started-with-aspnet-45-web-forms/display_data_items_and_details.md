---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Visualizzazione dei dati degli elementi e i dettagli | Documenti Microsoft
author: Erikre
description: Questa serie di esercitazioni verranno fornite le nozioni di base della creazione di un'applicazione Web Form ASP.NET tramite ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per abbiamo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 5fea654aa5116193cb7496c1b9020ed8e25fc06f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892569"
---
<a name="display-data-items-and-details"></a>Visualizzazione dei dati degli elementi e i dettagli
====================
da [Erik Reitan](https://github.com/Erikre)

[Scarica progetto di esempio Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [scaricare E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Questa serie di esercitazioni verranno fornite le nozioni di base della creazione di un'applicazione Web Form ASP.NET utilizzando ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per Web. Un Visual Studio 2013 [progetto con il codice sorgente c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) complemento questa serie di esercitazioni è disponibile.


In questa esercitazione viene descritto come visualizzare gli elementi di dati e i dettagli dell'elemento di dati con Entity Framework Code First e Web Form ASP.NET. In questa esercitazione si basa sull'esercitazione precedente "Dell'interfaccia utente e la navigazione" e fa parte della serie di esercitazioni Wingtip giocattoli archivio. Dopo aver completato questa esercitazione, sarà possibile visualizzare i prodotti sul *ProductsList.aspx* pagina e informazioni dettagliate su un singolo prodotto sul *ProductDetails.aspx* pagina.

## <a name="what-youll-learn"></a>Illustra quanto segue:

- Come aggiungere un controllo di dati per visualizzare i prodotti dal database.
- Come connettere un controllo dati per i dati selezionati.
- Come aggiungere un controllo di dati per visualizzare i dettagli sul prodotto dal database.
- Come recuperare un valore dalla stringa di query e utilizzare tale valore per limitare i dati recuperati dal database.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Le funzionalità introdotte in questa esercitazione sono:

- Associazione di modelli
- Provider di valori

## <a name="adding-a-data-control-to-display-products"></a>Aggiunta di un controllo di dati per visualizzare i prodotti

Quando si associano dati a un controllo server, esistono alcune opzioni diverse, che è possibile utilizzare. Le opzioni più comuni includono l'aggiunta di un controllo origine dati, aggiungere manualmente il codice o utilizzando l'associazione di modelli.

### <a name="using-a-data-source-control-to-bind-data"></a>Utilizzo di un controllo origine dati per associare i dati

Aggiunta di un controllo origine dati consente di collegare il controllo origine dati per il controllo che visualizza i dati. Questo approccio consente di connettersi in modo dichiarativo i controlli sul lato server direttamente a origini dati, anziché utilizzare un approccio a livello di codice.

### <a name="coding-by-hand-to-bind-data"></a>Per associare i dati di codifica manualmente

Aggiunta di codice a mano implica leggere il valore, verifica di un valore null, il tentativo di convertirlo nel tipo appropriato, verifica se la conversione ha avuto esito positivo e infine, utilizzare il valore nella query. Utilizzare questo approccio quando è necessario mantenere il controllo completo sulla logica di accesso ai dati.

### <a name="using-model-binding-to-bind-data"></a>Utilizzando l'associazione per associare i dati del modello

Utilizzando l'associazione di modelli consente di associare i risultati utilizzando molto meno codice e offre la possibilità di riutilizzare la funzionalità in tutta l'applicazione. Associazione modello mira a semplificano l'utilizzo con la logica di accesso ai dati focalizzato sul codice, pur mantenendo i vantaggi di un framework completo e l'associazione dati.

## <a name="displaying-products"></a>Visualizzazione dei prodotti

In questa esercitazione si userà l'associazione di modelli per associare i dati. Per configurare un controllo di dati per l'utilizzo di associazione di modelli per selezionare i dati, impostare il controllo `SelectMethod` proprietà sul nome di un metodo nel codice della pagina. Il controllo dati chiama il metodo nel momento adeguato del ciclo di vita della pagina e associa automaticamente i dati restituiti. Non è necessario chiamare in modo esplicito il `DataBind` metodo.

Seguendo questa procedura, verrà modificato il markup di *ProductList.aspx* pagina in modo che la pagina è possibile visualizzare i prodotti.

1. In **Esplora**, aprire il *ProductList.aspx* pagina.
2. Sostituire il codice esistente con il markup seguente:   

    [!code-aspx[Main](display_data_items_and_details/samples/sample1.aspx)]

Questo codice Usa un **ListView** controllo denominato "productList" per visualizzare i prodotti.

[!code-aspx[Main](display_data_items_and_details/samples/sample2.aspx)]

Il **ListView** controllo Visualizza i dati in un formato definito mediante modelli e stili. È utile per i dati in una struttura ripetuta. Questo **ListView** viene semplicemente i dati dal database, tuttavia è possibile abilitare agli utenti di modificare, inserire ed eliminare dati e per ordinare e dati di pagina, tutto senza scrivere codice.

Impostando il `ItemType` proprietà il **ListView** di controllo, espressione di associazione dati `Item` è disponibile e il controllo diventa fortemente tipizzato. Come accennato nell'esercitazione precedente, è possibile selezionare i dettagli dell'elemento oggetto usando IntelliSense, ad esempio specificando il `ProductName`:

![Visualizzare i dati elementi e dettagli - IntelliSense](display_data_items_and_details/_static/image1.png)

Inoltre, si utilizza l'associazione di modelli per specificare un `SelectMethod` valore. Questo valore (`GetProducts`) corrisponderà al metodo che verrà aggiunto al codice sottostante per visualizzare i prodotti nel passaggio successivo.

### <a name="adding-code-to-display-products"></a>Aggiungere il codice per visualizzare i prodotti

In questo passaggio verrà aggiunto il codice per popolare il **ListView** controllo con i dati prodotti dal database. Il codice supporterà con i prodotti da una singola categoria, nonché visualizzare tutti i prodotti.

1. In **Esplora**, fare doppio clic su *ProductList.aspx* e quindi fare clic su **Visualizza codice**.
2. Sostituire il codice esistente nel *ProductList.aspx.cs* file con il codice seguente:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

Questo codice viene illustrato il `GetProducts` metodo a cui fa riferimento il `ItemType` proprietà del **ListView** controllo il *ProductList* pagina. Per limitare i risultati a una categoria specifica nel database, il codice imposta il `categoryId` valore dal valore della stringa di query passato al *ProductList.aspx* pagina quando il *ProductList* pagina spostarsi su. Il `QueryStringAttribute` classe il `System.Web.ModelBinding` dello spazio dei nomi viene utilizzato per recuperare il valore dell'id di variabile di stringa di query. Questo indica l'associazione del modello per tentare di associare un valore dalla stringa di query per il `categoryId` parametro in fase di esecuzione.

Quando una categoria valida viene passata come stringa di query alla pagina, i risultati della query sono limitati a tali prodotti nel database che corrispondono il `categoryId` valore. Ad esempio, se l'URL per il *ProductsList.aspx* pagina è il seguente:

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

Nella pagina vengono visualizzati solo i prodotti in cui il `category` è uguale a `1`.

Se nessuna stringa di query viene inclusa durante la navigazione verso il *ProductList.aspx* pagina verranno visualizzati tutti i prodotti.

Le origini dei valori di questi metodi sono detti *valore provider* (ad esempio *QueryString*), e gli attributi di parametro che indicano quali provider di valori da utilizzare sono dette valore gli attributi del provider (ad esempio "`id`"). ASP.NET include i provider di valori e attributi corrispondenti per tutte le origini tipiche dell'input utente in un'applicazione Web Form, ad esempio la stringa di query, cookie, i valori del form, controlli, lo stato di visualizzazione, lo stato della sessione e le proprietà del profilo. È anche possibile scrivere provider di valori personalizzati.

### <a name="running-the-application"></a>Esecuzione dell'applicazione

Eseguire l'applicazione per vedere come è possibile visualizzare tutti i prodotti o un set di prodotti limitato in base alla categoria.

1. Nel **Esplora**, fare doppio clic su di *Default.aspx* pagina e selezionare **Visualizza nel Browser**.  
 Aprire il browser e visualizzare il *Default.aspx* pagina.
2. Selezionare **automobili** dal menu di navigazione di categoria di prodotto.  
 Il *ProductList.aspx* viene visualizzata la pagina che mostra solo i prodotti inclusi nella categoria "Auto". Più avanti in questa esercitazione, verranno visualizzati i dettagli sul prodotto.  

    ![Visualizzare i dati elementi e dettagli - automobili](display_data_items_and_details/_static/image2.png)
3. Selezionare **prodotti** dal menu di navigazione nella parte superiore.  
 Nuovamente, il *ProductList.aspx* viene visualizzata la pagina, ma questa volta viene illustrato l'intero elenco di prodotti.   

    ![Visualizzare i dati elementi e dettagli - prodotti](display_data_items_and_details/_static/image3.png)
4. Chiudere il browser e tornare a Visual Studio.

### <a name="adding-a-data-control-to-display-product-details"></a>Aggiunta di un controllo di dati per visualizzare i dettagli sul prodotto

Successivamente, che si desidera modificare il markup di *ProductDetails.aspx* pagina aggiunto nell'esercitazione precedente in modo che la pagina è possibile visualizzare informazioni su un singolo prodotto.

1. In **Esplora**, aprire il *ProductDetails.aspx* pagina.
2. Sostituire il codice esistente con il markup seguente:   

    [!code-aspx[Main](display_data_items_and_details/samples/sample5.aspx)]

Questo codice Usa un **FormView** controllo per visualizzare informazioni dettagliate su un singolo prodotto. In questo markup utilizza metodi, ad esempio quelli utilizzati per visualizzare i dati nel *ProductList.aspx* pagina. Il **FormView** controllo viene utilizzato per visualizzare un singolo record alla volta da un'origine dati. Quando si utilizza il **FormView** controllo, creazione di modelli per visualizzare e modificare i valori associati a dati. I modelli contengono controlli, formattazione e associazione di espressioni che definiscono l'aspetto e funzionalità del modulo.

Per connettere il markup precedente del database, è necessario aggiungere il codice aggiuntivo per il *ProductDetails.aspx* codice.

1. In **Esplora**, fare doppio clic su *ProductDetails.aspx* e quindi fare clic su **Visualizza codice**.  
   Il *ProductDetails.aspx.cs* file verrà visualizzato.
2. Sostituire il codice esistente con il seguente:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

Questo codice di verifica per un "`productID`" valore di stringa di query. Se viene trovato un valore di stringa di query valida, viene visualizzato il prodotto corrispondente. Se non viene trovata alcuna stringa di query o il valore di stringa di query non è valido, nessun prodotto viene visualizzato sul *ProductDetails.aspx* pagina.

### <a name="running-the-application"></a>Esecuzione dell'applicazione

Ora è possibile eseguire l'applicazione per visualizzare un singolo prodotto visualizzato in base all'id del prodotto.

1. Premere **F5** mentre in Visual Studio per eseguire l'applicazione.  
 Aprire il browser e visualizzare il *Default.aspx* pagina.
2. Selezionare "Emergenza" dal menu di navigazione di categoria.  
 Il *ProductList.aspx* viene visualizzata la pagina.
3. Selezionare il prodotto "Carta barca" dall'elenco di prodotti.  
 Il *ProductDetails.aspx* viene visualizzata la pagina.   

    ![Visualizzare i dati elementi e dettagli - prodotti](display_data_items_and_details/_static/image4.png)
4. Chiudere il browser.

## <a name="summary"></a>Riepilogo

In questa esercitazione della serie è necessario aggiungere markup e il codice per visualizzare un elenco di prodotti e visualizzare i dettagli sul prodotto. Durante questo processo si è appreso sui controlli di dati fortemente tipizzati, l'associazione di modelli e i provider di valori. Nella prossima esercitazione, si aggiungerà un carrello acquisti all'applicazione di esempio Wingtip Toys.

## <a name="additional-resources"></a>Risorse aggiuntive

[Il recupero e visualizzazione dei dati con l'associazione di modelli e web form](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> [Precedente](ui_and_navigation.md)
> [Successivo](shopping-cart.md)
