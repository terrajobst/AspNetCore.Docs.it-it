---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Visualizzare i dati degli elementi e i dettagli | Microsoft Docs
author: Erikre
description: Questa serie di esercitazioni insegnerà le nozioni di base della creazione di un'applicazione Web Form ASP.NET con ASP.NET 4.7 e Microsoft Visual Studio Community 2017 per il Web
ms.author: riande
ms.date: 1/09/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 73ae1660f5d6e3e28c1c155e745a62936e3502df
ms.sourcegitcommit: cec77d5ad8a0cedb1ecbec32834111492afd0cd2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207434"
---
<a name="display-data-items-and-details"></a>Elementi di dati visualizzati e dettagli
====================
da [Erik Reitan](https://github.com/Erikre)

> Questa serie di esercitazioni illustra le nozioni di base della creazione di un'applicazione Web Form ASP.NET con ASP.NET 4.7 e Microsoft Visual Studio Community 2017 per il Web.

In questa esercitazione descrive come visualizzare gli elementi di dati e i dettagli dell'elemento dati con Entity Framework Code First e Web Form ASP.NET. Questa esercitazione si basa sull'esercitazione precedente "Navigazione dell'interfaccia utente e" come parte della serie di esercitazioni di Wingtip giocattoli Store. Nell'esercitazione completata, i prodotti nel *ProductsList.aspx* pagina e i dettagli di un prodotto nel *ProductDetails.aspx* pagina vengono visualizzati.

## <a name="what-you-learn"></a>Tutto ciò che Apprendi

- Aggiungere un controllo dei dati per visualizzare i prodotti di database.
- Connettere un controllo dei dati per i dati selezionati.
- Aggiungere un controllo dei dati per visualizzare i dettagli del prodotto.
- Analizzare un valore di stringa di query e usarlo per filtrare i dati del database recuperato.

Le funzionalità introdotte in questa esercitazione includono l'associazione di modelli e i provider di valori.

## <a name="add-a-data-control-to-display-products"></a>Aggiungere un controllo dati per visualizzare i prodotti
 
Sono disponibili alcune opzioni per associare dati a un controllo server. I più comuni includono:

 * Aggiunta di un controllo origine dati
 * Aggiunta manuale di codice
 * Implementazione di associazione di modelli

### <a name="use-a-data-source-control-to-bind-data"></a>Usare un controllo origine dati per associare i dati

Aggiunta di un controllo origine dati, i collegamenti al controllo origine dati al controllo che visualizza i dati. Con questo approccio, è possibile in modo dichiarativo, anziché connettersi a livello di codice i controlli lato server alle origini dati.

### <a name="code-by-hand-to-bind-data"></a>Codice manualmente per associare i dati

Codifica manuale prevede:

1. Lettura di un valore
2. Controllare se è null
3. Conversione in un tipo appropriato
4. Controllo operazioni riuscite di conversione
5. Effettua una query con il valore convertito 

Con questo approccio, è necessario il controllo completo per la logica di accesso ai dati.

### <a name="use-model-binding-to-bind-data"></a>Usare l'associazione di modelli per associare i dati

Con l'associazione di modelli, associare i risultati con molto meno codice e offre la possibilità di riusare le funzionalità in tutta l'applicazione. Semplifica l'utilizzo con la logica di accesso ai dati incentrato sul codice garantendo comunque un framework completo e associazione dati.

## <a name="display-products"></a>Visualizzare i prodotti

In questa esercitazione si usa l'associazione di modelli per associare i dati. Per configurare un controllo dati per utilizzare l'associazione di modelli per selezionare i dati, si imposta il controllo `SelectMethod` proprietà a un metodo nel codice della pagina. Il controllo dei dati chiama il metodo momento adeguato del ciclo di vita della pagina e associa automaticamente i dati restituiti. Non è necessario chiamare in modo esplicito il `DataBind` (metodo).

Usa i passaggi seguenti, si modificano *ProductList. aspx* markup per visualizzare i prodotti.

1. Nelle **Esplora soluzioni**aprire *ProductList. aspx*.

2. Sostituire il markup esistente con il markup seguente: 

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

Il markup precedente Usa un **ListView** controllo denominato `productList` per visualizzare i prodotti.

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

Con stili e modelli, è possibile definire come il **ListView** controllo Visualizza i dati. È utile per i dati in una struttura ripetuta. Anche se ciò **ListView** viene visualizzata semplicemente i dati del database, è possibile anche, senza codice, consentire agli utenti di modificare, inserire ed eliminare dati e di ordinamento e paging dei dati.

Quando si imposta la `ItemType` proprietà nel **ListView** controllare, l'espressione di associazione dati `Item` è disponibile e il controllo diventa fortemente tipizzato. Come indicato nell'esercitazione precedente, è possibile selezionare i dettagli dell'oggetto elemento con la tecnologia IntelliSense, ad esempio specificando la `ProductName`:

![Visualizzare i dati gli elementi e i dettagli - IntelliSense](display_data_items_and_details/_static/image1.png)

Con l'associazione di modelli, si specifica un `SelectMethod` valore (`GetProducts`). Questo è il metodo da aggiungere al codice indietro per visualizzare i prodotti nel passaggio successivo.

### <a name="add-code-to-display-products"></a>Aggiungere codice per visualizzare i prodotti

In questo passaggio si aggiunge codice per popolare la **ListView** controllo con i dati del prodotto di database. Il codice supporta che mostra tutti i prodotti e singola categoria.

1. Nelle **Esplora soluzioni**, fare doppio clic su *ProductList. aspx* e quindi selezionare **Visualizza codice**.
2. Sostituire il codice esistente nel *ProductList.aspx.cs* file con questo:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

Questo codice viene illustrato il `GetProducts` metodo che i **ListView** del controllo `ItemType` riferimenti alla proprietà nella *ProductList. aspx*. Per limitare i risultati a una categoria di database specifico, il codice imposta la `categoryId` valore dalla stringa di query passata a *ProductList. aspx*. Il `QueryStringAttribute` classe la `System.Web.ModelBinding` dello spazio dei nomi viene usato per recuperare la variabile di stringa di query `id`del valore. Ciò indica l'associazione di modelli per, in fase di esecuzione, associare un valore di stringa di query per il `categoryId` parametro.

Quando una categoria valida (`categoryId`) viene passato, i risultati sono limitati ai prodotti del database della categoria. Ad esempio, se il *ProductsList.aspx* URL della pagina è questo:

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

Nella pagina vengono visualizzati solo i prodotti in cui il `categoryId` è uguale a `1`.

Se non viene passata alcuna stringa di query, verranno visualizzati tutti i prodotti.

Le origini dei valori per questi metodi vengono chiamate *valore provider* (ad esempio `QueryString`), e gli attributi di parametro che indicano quali provider di valori da utilizzare vengono chiamati *attributi di provider di valore* ( ad esempio `id`). ASP.NET include i provider di valori e gli attributi per tutti i Web Form dell'applicazione utente inpue origini tipiche. Queste includono la stringa di query, cookie, i valori del form, controlli, lo stato di visualizzazione, lo stato della sessione e le proprietà del profilo. È anche possibile scrivere provider di valori personalizzati.

### <a name="run-the-application"></a>Esecuzione dell'applicazione

Eseguire ora l'applicazione per visualizzare tutti i prodotti o i prodotti della categoria.

1. In Visual Studio, premere **F5** per eseguire l'applicazione.
 Il browser si apre e Mostra le *default. aspx* pagina.

2. Nel menu di categoria di prodotto, selezionare **automobili**.

   Il *ProductList. aspx* verrà visualizzata la pagina, che mostra solo i prodotti dalle **automobili** categoria. Più avanti in questa esercitazione è visualizzare i dettagli del prodotto.

    ![Visualizzare i dati gli elementi e i dettagli - automobili](display_data_items_and_details/_static/image2.png)

3. Selezionare **prodotti** dal menu in alto.
 Il *ProductList. aspx* nella pagina verranno visualizzati tutti i prodotti. 

    ![Visualizzare i dati gli elementi e i dettagli - prodotti](display_data_items_and_details/_static/image3.png)

4. Chiudere il browser e tornare a Visual Studio.

### <a name="add-a-data-control-to-display-product-details"></a>Aggiungere un controllo dei dati per visualizzare i dettagli sul prodotto

Modificare il *ProductDetails.aspx* markup di aggiunto nell'esercitazione precedente per visualizzare le informazioni di prodotto specifico:

1. Nelle **Esplora soluzioni**aprire *ProductDetails.aspx*.

2. Sostituire il markup esistente con questo codice:

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

Questo markup Usa un' **FormView** controllo per visualizzare i dettagli sul prodotto specifico. Usa i metodi, ad esempio quelle usate per visualizzare i dati in *ProductList. aspx*. Il **FormView** controllo viene usato per visualizzare un singolo record alla volta da un'origine dati. Quando si usa la **FormView** (controllo), è creare modelli per visualizzare e modificare i valori associati a dati. Questi modelli contengono controlli, espressioni di associazione, e formattazione che definiscono l'aspetto e funzionalità del modulo.

Il markup precedente la connessione al database richiede codice aggiuntivo.

1. Nelle **Esplora soluzioni**, fare doppio clic su *ProductDetails.aspx* e quindi selezionare **Visualizza codice**.  
   Il *ProductDetails.aspx.cs* file viene visualizzato.

2. Sostituire il codice esistente con questo:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

Questo codice cerca un "`productID`" valore di stringa di query. Se viene trovato un valore valido, viene visualizzato il prodotto corrisponda. Se non viene trovata la stringa di query o il relativo valore non è valido, non viene visualizzato alcun prodotto.

### <a name="run-the-application"></a>Esecuzione dell'applicazione

A questo punto è possibile eseguire l'applicazione per vedere i dettagli sul prodotto specifico basati su ID prodotto.

1. In Visual Studio, premere **F5** per eseguire l'applicazione.  
 Nel browser verrà aperta *default. aspx*.

2. Nel menu di categoria, selezionare **evolveranno**.  
 Il *ProductList. aspx* viene visualizzata la pagina.

3. Selezionare **carta barca**.  
 Il *ProductDetails.aspx* viene visualizzata la pagina.   

    ![Visualizzare i dati gli elementi e i dettagli - prodotti](display_data_items_and_details/_static/image4.png)

Nella prossima esercitazione, un carrello acquisti è aggiungere l'applicazione Wingtip Toys.

## <a name="additional-resources"></a>Risorse aggiuntive

[Il recupero e visualizzazione dei dati con l'associazione di modelli e web form](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> [Precedente](ui_and_navigation.md)
> [Successivo](shopping-cart.md)
