---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Visualizzare i dati degli elementi e i dettagli | Microsoft Docs
author: Erikre
description: Questa serie di esercitazioni insegnerà le nozioni di base della creazione di un'applicazione Web Form ASP.NET con ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per Microsoft...
ms.author: aspnetcontent
ms.date: 09/08/2014
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 083f7182416012c85f05db255fcab4d8e535b52a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820347"
---
<a name="display-data-items-and-details"></a>Visualizzare i dati degli elementi e i dettagli
====================
da [Erik Reitan](https://github.com/Erikre)

[Scaricare progetto di esempio Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [Scarica l'E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Questa serie di esercitazioni insegnerà le nozioni di base della creazione di un'applicazione Web Form ASP.NET con ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per Web. Un Visual Studio 2013 [progetto con codice sorgente c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) complemento a questa serie di esercitazioni è disponibile.


Questa esercitazione descrive come visualizzare gli elementi di dati e i dettagli dell'elemento dati utilizzando Web Form ASP.NET e Code First di Entity Framework. Questa esercitazione si basa sull'esercitazione precedente "E navigazione nell'interfaccia utente" e fa parte della serie di esercitazioni di Wingtip giocattoli Store. Dopo aver completato questa esercitazione, sarà possibile visualizzare i prodotti sul *ProductsList.aspx* pagina e informazioni dettagliate su un singolo prodotto sulle *ProductDetails.aspx* pagina.

## <a name="what-youll-learn"></a>Che cosa si apprenderà come:

- Come aggiungere un controllo dati per visualizzare i prodotti dal database.
- Come connettere un controllo dati per i dati selezionati.
- Come aggiungere un controllo dati per visualizzare i dettagli sul prodotto dal database.
- Come recuperare un valore dalla stringa di query e usare tale valore per limitare i dati recuperati dal database.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Queste sono le funzionalità introdotte in questa esercitazione:

- Associazione di modelli
- Provider di valori

## <a name="adding-a-data-control-to-display-products"></a>Aggiunta di un controllo dei dati per visualizzare i prodotti

Quando si associano dati a un controllo server, esistono alcune opzioni diverse, che è possibile usare. Le opzioni più comuni includono l'aggiunta di un controllo origine dati, aggiunta manuale di codice o utilizzando l'associazione di modelli.

### <a name="using-a-data-source-control-to-bind-data"></a>Utilizzo di un controllo origine dati per l'associazione dati

Aggiunta di un controllo origine dati consente di collegare il controllo origine dati al controllo che visualizza i dati. Questo approccio consente di connettersi in modo dichiarativo i controlli lato server direttamente a origini dati, anziché usare un approccio programmatico.

### <a name="coding-by-hand-to-bind-data"></a>Codifica manuale per associare i dati

Aggiungendo codice rimboccarsi comporta la lettura del valore, verifica di un valore null, si prova a convertirlo nel tipo appropriato, verifica se la conversione ha avuto esito positivo e infine, utilizza il valore nella query. Utilizzare questo approccio quando è necessario mantenere il controllo completo per la logica di accesso ai dati.

### <a name="using-model-binding-to-bind-data"></a>Uso del modello di associazione per associare i dati

Usando l'associazione di modelli consente di associare i risultati usando molto meno codice e ti offre la possibilità di riusare le funzionalità in tutta l'applicazione. Associazione modello mira a semplificano l'uso con la logica di accesso ai dati incentrato sul codice, pur mantenendo i vantaggi di un framework completo e associazione dati.

## <a name="displaying-products"></a>Visualizzazione dei prodotti

In questa esercitazione si userà l'associazione di modelli per associare i dati. Per configurare un controllo dati per utilizzare l'associazione di modelli per selezionare i dati, si imposta il controllo `SelectMethod` proprietà sul nome di un metodo nel codice della pagina. Il controllo dei dati chiama il metodo momento adeguato del ciclo di vita della pagina e associa automaticamente i dati restituiti. Non è necessario chiamare in modo esplicito il `DataBind` (metodo).

Seguendo questa procedura, si modificherà il markup nel *ProductList. aspx* pagina in modo che la pagina è possibile visualizzare i prodotti.

1. Nelle **Esplora soluzioni**, aprire il *ProductList. aspx* pagina.
2. Sostituire il markup esistente con il markup seguente:   

    [!code-aspx[Main](display_data_items_and_details/samples/sample1.aspx)]

Questo codice Usa un **ListView** controllo denominato "productList" per visualizzare i prodotti.

[!code-aspx[Main](display_data_items_and_details/samples/sample2.aspx)]

Il **ListView** controllo Visualizza i dati in un formato definite tramite stili e modelli. È utile per i dati in una struttura ripetuta. Ciò **ListView** esempio mostra semplicemente i dati dal database, tuttavia è possibile abilitare gli utenti per modificare, inserire ed eliminare i dati e per ordinare e dati di pagina, tutto senza codice.

Impostando il `ItemType` proprietà nel **ListView** controllare, l'espressione di associazione dati `Item` è disponibile e il controllo diventa fortemente tipizzato. Come indicato nell'esercitazione precedente, è possibile selezionare i dettagli dell'oggetto elemento usando IntelliSense, ad esempio specificando la `ProductName`:

![Visualizzare i dati gli elementi e i dettagli - IntelliSense](display_data_items_and_details/_static/image1.png)

Inoltre, si usa l'associazione di modelli per specificare un `SelectMethod` valore. Questo valore (`GetProducts`) corrisponderà al metodo che si aggiungerà al code-behind per visualizzare i prodotti nel passaggio successivo.

### <a name="adding-code-to-display-products"></a>Aggiunta di codice per visualizzare i prodotti

In questo passaggio si aggiungerà codice per popolare la **ListView** controllo con i dati prodotti dal database. Il codice supporterà che mostra i prodotti da singola categoria, nonché visualizzare tutti i prodotti.

1. Nelle **Esplora soluzioni**, fare doppio clic su *ProductList. aspx* e quindi fare clic su **Visualizza codice**.
2. Sostituire il codice esistente nel *ProductList.aspx.cs* file con il codice seguente:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

Questo codice viene illustrato il `GetProducts` metodo che fa riferimento il `ItemType` proprietà delle **ListView** controllare nel *ProductList. aspx* pagina. Per limitare i risultati a una categoria specifica nel database, il codice imposta la `categoryId` valore del valore di stringa di query passato al *ProductList. aspx* pagina quando la *ProductList. aspx* pagina è ci si sposta su. Il `QueryStringAttribute` classe di `System.Web.ModelBinding` dello spazio dei nomi viene usato per recuperare il valore dell'id di variabili di stringa di query. Ciò indica l'associazione di modelli per tentare di associare un valore di stringa di query di `categoryId` parametro in fase di esecuzione.

Quando una categoria valida viene passata come stringa di query alla pagina, i risultati della query sono limitati a tali prodotti nel database che corrispondono al `categoryId` valore. Ad esempio, se l'URL per il *ProductsList.aspx* pagina è il seguente:

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

Nella pagina vengono visualizzati solo i prodotti in cui il `category` è uguale a `1`.

Se non è inclusa alcuna stringa di query durante la navigazione verso il *ProductList. aspx* pagina verranno visualizzati tutti i prodotti.

Le origini dei valori per questi metodi sono dette *valore provider* (ad esempio *QueryString*), e gli attributi di parametro che indicano quali provider di valori da utilizzare sono dette valore gli attributi del provider (ad esempio "`id`"). ASP.NET include i provider di valori e gli attributi corrispondenti per tutte le origini tipiche dell'input utente in un'applicazione Web Form, ad esempio la stringa di query, cookie, i valori del form, controlli, lo stato di visualizzazione, lo stato della sessione e le proprietà del profilo. È anche possibile scrivere provider di valori personalizzati.

### <a name="running-the-application"></a>Esecuzione dell'applicazione

Eseguire ora l'applicazione per vedere come è possibile visualizzare tutti i prodotti o semplicemente un set di prodotti limitate in base alla categoria.

1. Nel **Esplora soluzioni**, fare doppio clic il *default. aspx* pagina e selezionare **Visualizza nel Browser**.  
 Verrà aperto il browser e visualizzare il *default. aspx* pagina.
2. Selezionare **automobili** dal menu di navigazione di categoria prodotto.  
 Il *ProductList. aspx* viene visualizzata la pagina che mostra solo i prodotti inclusi nella categoria "Cars". Più avanti in questa esercitazione, si visualizzerà i dettagli del prodotto.  

    ![Visualizzare i dati gli elementi e i dettagli - automobili](display_data_items_and_details/_static/image2.png)
3. Selezionare **prodotti** dal menu di navigazione nella parte superiore.  
 Anche in questo caso, il *ProductList. aspx* pagina viene visualizzata, ma questa volta Mostra l'elenco completo di prodotti.   

    ![Visualizzare i dati gli elementi e i dettagli - prodotti](display_data_items_and_details/_static/image3.png)
4. Chiudere il browser e tornare a Visual Studio.

### <a name="adding-a-data-control-to-display-product-details"></a>Aggiunta di un controllo dei dati per visualizzare i dettagli sul prodotto

Successivamente, si modificherà il markup nel *ProductDetails.aspx* pagina in cui è stato aggiunto nell'esercitazione precedente in modo che la pagina è possibile visualizzare informazioni su un singolo prodotto.

1. Nelle **Esplora soluzioni**, aprire il *ProductDetails.aspx* pagina.
2. Sostituire il markup esistente con il markup seguente:   

    [!code-aspx[Main](display_data_items_and_details/samples/sample5.aspx)]

Questo codice Usa un **FormView** controllo per visualizzare informazioni dettagliate su un singolo prodotto. Questo markup Usa metodi simili a quelli utilizzati per visualizzare i dati nella *ProductList. aspx* pagina. Il **FormView** controllo viene usato per visualizzare un singolo record alla volta da un'origine dati. Quando si usa la **FormView** (controllo), è creare modelli per visualizzare e modificare i valori associati a dati. I modelli contengono controlli, espressioni di associazione e la formattazione che definiscono l'aspetto e funzionalità del modulo.

Per connettere il markup precedente al database, è necessario aggiungere il codice aggiuntivo per il *ProductDetails.aspx* codice.

1. Nelle **Esplora soluzioni**, fare doppio clic su *ProductDetails.aspx* e quindi fare clic su **Visualizza codice**.  
   Il *ProductDetails.aspx.cs* verrà visualizzati file.
2. Sostituire il codice esistente con il seguente:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

Questo codice cerca un "`productID`" valore di stringa di query. Se viene trovato un valore di stringa di query valida, viene visualizzato il prodotto corrisponda. Se non viene trovata alcuna stringa di query o il valore di stringa di query non è valido, nessun prodotto viene visualizzato nei *ProductDetails.aspx* pagina.

### <a name="running-the-application"></a>Esecuzione dell'applicazione

A questo punto è possibile eseguire l'applicazione per visualizzare un singolo prodotto visualizzato in base all'id del prodotto.

1. Premere **F5** all'interno di Visual Studio per eseguire l'applicazione.  
 Verrà aperto il browser e visualizzare il *default. aspx* pagina.
2. Selezionare "Evolveranno" dal menu di navigazione di categoria.  
 Il *ProductList. aspx* viene visualizzata la pagina.
3. Selezionare il prodotto "Paper barca" dall'elenco dei prodotti.  
 Il *ProductDetails.aspx* viene visualizzata la pagina.   

    ![Visualizzare i dati gli elementi e i dettagli - prodotti](display_data_items_and_details/_static/image4.png)
4. Chiudere il browser.

## <a name="summary"></a>Riepilogo

In questa esercitazione della serie si sono aggiunte markup e codice per visualizzare un elenco di prodotti e visualizzare i dettagli sul prodotto. Durante questo processo si è appreso sui controlli dati fortemente tipizzati, l'associazione di modelli e i provider di valori. Nella prossima esercitazione, si aggiungerà un carrello della spesa per l'applicazione di esempio Wingtip Toys.

## <a name="additional-resources"></a>Risorse aggiuntive

[Il recupero e visualizzazione dei dati con l'associazione di modelli e web form](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> [Precedente](ui_and_navigation.md)
> [Successivo](shopping-cart.md)
