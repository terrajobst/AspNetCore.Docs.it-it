---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
title: Carrello della spesa | Microsoft Docs
author: Erikre
description: Questa serie di esercitazioni insegnerà le nozioni di base della creazione di un'applicazione Web Form ASP.NET con ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per Microsoft...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 6898c601-6c31-432f-8388-e6843f8a17cb
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
msc.type: authoredcontent
ms.openlocfilehash: 22253b8708efd9ce505c9fbeb9cb2e942588b37d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375778"
---
<a name="shopping-cart"></a>Carrello della spesa
====================
da [Erik Reitan](https://github.com/Erikre)

[Scaricare progetto di esempio Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [Scarica l'E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Questa serie di esercitazioni insegnerà le nozioni di base della creazione di un'applicazione Web Form ASP.NET con ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per Web. Un Visual Studio 2013 [progetto con codice sorgente c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) complemento a questa serie di esercitazioni è disponibile.


Questa esercitazione illustra la logica di business richiesta per aggiungere un carrello della spesa per l'applicazione Web Form ASP.NET di esempio Wingtip Toys. Questa esercitazione si basa sull'esercitazione precedente "Visualizzazione dei dati e i dettagli degli elementi" e fa parte della serie di esercitazioni di Wingtip giocattoli Store. Dopo aver completato questa esercitazione, gli utenti dell'app di esempio saranno in grado di aggiungere, rimuovere e modificare i prodotti nel carrello.

## <a name="what-youll-learn"></a>Che cosa si apprenderà come:

1. Come creare un carrello della spesa per l'applicazione web.
2. Come consentire agli utenti di aggiungere elementi al carrello acquisti.
3. Come aggiungere un [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview(v=vs.110).aspx#introduction) controllo per visualizzare i dettagli del carrello acquisti.
4. Come calcolare e visualizzare il totale degli ordini.
5. Come rimuovere e aggiornare gli elementi nel carrello acquisti.
6. Come includere un contatore di carrello degli acquisti.

## <a name="code-features-in-this-tutorial"></a>Funzionalità di codice in questa esercitazione:

1. Entity Framework Code First
2. Annotazioni dei dati
3. Controlli dati fortemente tipizzati
4. Associazione di modelli

## <a name="creating-a-shopping-cart"></a>Creazione di un carrello acquisti

In precedenza in questa serie di esercitazioni, è stato aggiunto le pagine e il codice per visualizzare i dati prodotti da un database. In questa esercitazione si creerà un carrello acquisti per gestire i prodotti che gli utenti interessati all'acquisto. Gli utenti saranno in grado di individuare e aggiungere elementi al carrello acquisti, anche se non sono registrati o registrati. Per gestire l'accesso di carrello degli acquisti, verrà assegnato agli utenti un valore univoco `ID` utilizzando un identificatore univoco globale (GUID) quando l'utente accede il market carrello per la prima volta. Si archivierà `ID` usando lo stato della sessione ASP.NET.

> [!NOTE] 
> 
> Lo stato della sessione ASP.NET è un modo pratico per archiviare le informazioni specifiche dell'utente che scadranno dopo che l'utente ha lasciato il sito. Mentre un utilizzo improprio dello stato della sessione può avere implicazioni sulle prestazioni in siti di grandi dimensioni, chiaro di sessione stato funziona bene per scopi dimostrativi. Il progetto di esempio Wingtip Toys viene illustrato come utilizzare lo stato della sessione senza un provider esterno, in cui lo stato della sessione è archiviato in-process nel server web che ospita il sito. Per i siti di grandi dimensioni che forniscono più istanze di un'applicazione o per siti che eseguono più istanze di un'applicazione in server diversi, è consigliabile usare **il servizio Cache di Windows Azure**. Questo servizio Cache fornisce un servizio di memorizzazione nella cache distribuito che è esterno al sito web e consente di risolvere il problema dell'utilizzo dello stato sessione in-process. Per altre informazioni, vedere [come usare lo stato della sessione ASP.NET con siti Web di Azure](https://docs.microsoft.com/azure/redis-cache/cache-aspnet-session-state-provider).


### <a name="add-cartitem-as-a-model-class"></a>Aggiungere CartItem come classe di modello

In precedenza in questa serie di esercitazioni, è stato definito lo schema per i dati category e product creando il `Category` e `Product` le classi nelle *modelli* cartella. A questo punto, aggiungere una nuova classe per definire lo schema per il carrello acquisti. Più avanti in questa esercitazione si aggiungerà una classe per gestire l'accesso ai dati di `CartItem` tabella. Questa classe fornirà la logica di business per aggiungere, rimuovere e aggiornare gli elementi nel carrello acquisti.

1. Fare doppio clic il *modelli* cartella e selezionare **Add**  - &gt; **nuovo elemento**. 

    ![Il carrello della spesa - nuovo elemento](shopping-cart/_static/image1.png)
2. Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**. Selezionare **codice**, quindi selezionare **classe**. 

    ![Il carrello della spesa - Aggiungi finestra di dialogo Nuovo elemento](shopping-cart/_static/image2.png)
3. Nuova classe il nome *CartItem.cs*.
4. Fare clic su **Aggiungi**.  
   Il nuovo file di classe viene visualizzato nell'editor.
5. Sostituire il codice predefinito con il codice seguente:   

    [!code-csharp[Main](shopping-cart/samples/sample1.cs)]

Il `CartItem` classe contiene lo schema che definisce ogni prodotto un utente viene aggiunto al carrello acquisti. Questa classe è simile alle altre classi di schema creato in precedenza in questa serie di esercitazioni. Per convenzione, Code First di Entity Framework presuppone che la chiave primaria per la `CartItem` tabella può essere `CartItemId` o `ID`. Tuttavia, il codice esegue l'override del comportamento predefinito utilizzando l'annotazione dei dati `[Key]` attributo. Il `Key` attributo della proprietà ID dell'elemento specifica che il `ItemID` proprietà è la chiave primaria.

Il `CartId` proprietà specifica il `ID` dell'utente che è associato l'elemento da acquistare. Si aggiungerà codice per creare questo utente `ID` quando l'utente accede al carrello acquisti. Ciò `ID` verrà anche archiviata come variabile di sessione di ASP.NET.

### <a name="update-the-product-context"></a>Aggiornare il contesto del prodotto

Oltre ad aggiungere il `CartItem` (classe), sarà necessario aggiornare la classe del contesto del database che gestisce le classi di entità e che fornisce accesso ai dati nel database. A tale scopo, si aggiungeranno appena creato `CartItem` classe di modello il `ProductContext` classe.

1. Nel **Esplora soluzioni**individuare e aprire il *ProductContext.cs* del file nei *modelli* cartella.
2. Aggiungere il codice evidenziato per il *ProductContext.cs* file come segue:  

    [!code-csharp[Main](shopping-cart/samples/sample2.cs?highlight=14)]

Come accennato in precedenza in questa serie di esercitazioni, il codice nel *ProductContext.cs* file aggiunge il `System.Data.Entity` dello spazio dei nomi in modo da poter accedere a tutte le funzionalità core di Entity Framework. Questa funzionalità include la possibilità di eseguire una query, inserire, aggiornare ed eliminare dati utilizzando oggetti fortemente tipizzati. Il `ProductContext` classe aggiunge l'accesso a appena aggiunta `CartItem` classe del modello.

### <a name="managing-the-shopping-cart-business-logic"></a>Gestire la logica di Business di carrello degli acquisti

Successivamente, si creerà il `ShoppingCart` in una nuova classe *logica* cartella. Il `ShoppingCart` classe gestisce l'accesso ai dati di `CartItem` tabella. La classe includerà anche la logica di business per aggiungere, rimuovere e aggiornare gli elementi nel carrello acquisti.

La logica di carrello degli acquisti che verranno aggiunti conterrà la funzionalità per gestire le azioni seguenti:

1. Aggiunta di articoli al carrello acquisti
2. Rimozione di elementi del carrello
3. Ottenere l'ID carrello acquisti
4. Recupero degli elementi del carrello
5. Per un totale la quantità di tutti gli elementi del carrello acquisti
6. Aggiornamento dei dati del carrello acquisti

Una pagina carrello acquisti (*ShoppingCart.aspx*) e la classe di carrello degli acquisti verrà usata insieme per accedere ai dati del carrello acquisti. La pagina carrello degli acquisti verrà visualizzati tutti gli elementi che all'utente viene aggiunto al carrello acquisti. Oltre il carrello pagina e la classe, si creerà una pagina (*AddToCart.aspx*) per aggiungere prodotti al carrello acquisti. Si aggiungerà inoltre codice per il *ProductList. aspx* pagina e il *ProductDetails.aspx* pagina che fornirà un collegamento per il *AddToCart.aspx* pagina, in modo che l'utente può aggiungere prodotti per il carrello acquisti.

Il diagramma seguente illustra il processo di base che si verifica quando l'utente aggiunge un prodotto al carrello acquisti.

![Il carrello della spesa - aggiunta al carrello](shopping-cart/_static/image3.png)

Quando l'utente fa clic il **Add To Cart** collegamento in uno il *ProductList. aspx* pagina o la *ProductDetails.aspx* pagina, l'applicazione verrà visualizzata la *AddToCart.aspx* pagina e quindi automaticamente il *ShoppingCart.aspx* pagina. Il *AddToCart.aspx* pagina aggiungerà la selezione del prodotto al carrello acquisti chiamando un metodo nella classe ShoppingCart. Il *ShoppingCart.aspx* pagina visualizzerà i prodotti che sono state aggiunte al carrello acquisti.

#### <a name="creating-the-shopping-cart-class"></a>Creazione della classe di carrello degli acquisti

Il `ShoppingCart` classe verrà aggiunta in una cartella separata nell'applicazione in modo che sia una chiara distinzione tra il modello (cartella Models), le pagine (cartella radice) e la logica (cartella per la logica).

1. Nella **Esplora soluzioni**, fare doppio clic il **WingtipToys**del progetto e selezionare **Add**-&gt;**nuova cartella**. Denominare la nuova cartella *logica*.
2. Fare doppio clic il *per la logica* cartella e quindi selezionare **Add**  - &gt; **nuovo elemento**.
3. Aggiungere un nuovo file di classe denominato *ShoppingCartActions.cs*.
4. Sostituire il codice predefinito con il codice seguente:   

    [!code-csharp[Main](shopping-cart/samples/sample3.cs)]

Il `AddToCart` metodo consente a singoli prodotti da includere nel carrello acquisti in base al prodotto `ID`. Il prodotto viene aggiunto al carrello o se il carrello contiene già una voce per il prodotto, la quantità viene incrementata.

Il `GetCartId` metodo viene restituito il carrello `ID` per l'utente. Il carrello `ID` usata per rilevare gli elementi che un utente ha inseriti nel carrello. Se l'utente non dispone di un carrello della spesa esistente `ID`, un carrello nuovo `ID` viene immediatamente creato uno. Se l'utente ha effettuato l'accesso come utente registrato, il carrello `ID` è impostato su nome utente. Tuttavia, se l'utente non è connesso, il carrello `ID` è impostata su un valore univoco (GUID). Un GUID garantisce che solo un carrello della spesa viene creato per ogni utente, basato su sessione.

Il `GetCartItems` metodo restituisce un elenco di shopping cart voci dell'utente. Più avanti in questa esercitazione, si noterà che l'associazione di modelli utilizzato per visualizzare gli elementi del carrello nel carrello acquisti tramite il `GetCartItems` (metodo).

### <a name="creating-the-add-to-cart-functionality"></a>Creare la funzionalità Aggiungi al carrello

Come accennato in precedenza, si creerà una pagina di elaborazione denominata *AddToCart.aspx* che verrà usato per aggiungere nuovi prodotti per il carrello acquisti dell'utente. Questa pagina verrà chiamato il `AddToCart` metodo nel `ShoppingCart` classe appena creata. Il *AddToCart.aspx* pagina si aspetta che un prodotto `ID` viene passato al metodo. Questo prodotto `ID` verrà usato quando si chiama il `AddToCart` metodo nel `ShoppingCart` classe.

> [!NOTE] 
> 
> Verrà modificato il code-behind (*AddToCart.aspx.cs*) per questa pagina, non l'interfaccia utente della pagina (*AddToCart.aspx*).


#### <a name="to-create-the-add-to-cart-functionality"></a>Per creare l'Add To Cart funzionalità:

1. Nella **Esplora soluzioni**, fare doppio clic il **WingtipToys**del progetto, fare clic su **Add**  - &gt; **nuovo elemento**.  
   Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Aggiungere una nuova pagina standard (Web Form) per l'applicazione denominata *AddToCart.aspx*. 

    ![Il carrello della spesa - Aggiungi Web Form](shopping-cart/_static/image4.png)
3. Nelle **Esplora soluzioni**, fare doppio clic il *AddToCart.aspx* e quindi fare clic su **Visualizza codice**. Il *AddToCart.aspx.cs* file code-behind viene aperto nell'editor.
4. Sostituire il codice esistente nel *AddToCart.aspx.cs* code-behind con il codice seguente:   

    [!code-csharp[Main](shopping-cart/samples/sample4.cs)]

Quando la *AddToCart.aspx* pagina viene caricata, il prodotto `ID` viene recuperato dalla stringa di query. Successivamente, viene creata e usata per chiamare un'istanza della classe di carrello degli acquisti di `AddToCart` metodo aggiunto in precedenza in questa esercitazione. Il `AddToCart` contenuta nel metodo il *ShoppingCartActions.cs* file, include la logica per aggiungere il prodotto selezionato al carrello acquisti o incrementare la quantità del prodotto del prodotto selezionato. Se il prodotto non è ancora stato aggiunto al carrello acquisti, il prodotto viene aggiunto al `CartItem` tabella del database. Se il prodotto è già stato aggiunto al carrello acquisti e l'utente aggiunge un elemento aggiuntivo dello stesso prodotto, la quantità del prodotto viene incrementata nel `CartItem` tabella. Infine, la pagina viene reindirizzato al *ShoppingCart.aspx* pagina che verrà aggiunto nel passaggio successivo, in cui l'utente visualizza un elenco aggiornato degli elementi nel carrello.

Come accennato in precedenza, un utente `ID` viene usato per identificare i prodotti che sono associati a un utente specifico. Ciò `ID` viene aggiunto a una riga di `CartItem` tabella ogni volta che l'utente aggiunge un prodotto al carrello acquisti.

### <a name="creating-the-shopping-cart-ui"></a>Creazione del carrello degli acquisti dell'interfaccia utente

Il *ShoppingCart.aspx* pagina visualizzerà i prodotti che l'utente è aggiunto al carrello acquisti. Fornirà inoltre la possibilità di aggiungere, rimuovere e aggiornare gli elementi nel carrello acquisti.

1. Nelle **Esplora soluzioni**, fare doppio clic su **WingtipToys**, fare clic su **Add**  - &gt; **nuovo elemento**.  
   Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Aggiungere una nuova pagina (Web Form) che include una pagina master selezionando **Web Form mediante pagina Master**. Denominare la nuova pagina *ShoppingCart.aspx*.
3. Selezionare **Site. master** collegare la pagina master per l'oggetto appena creato *aspx* pagina.
4. Nel *ShoppingCart.aspx* pagina, sostituire il markup esistente con il markup seguente:   

    [!code-aspx[Main](shopping-cart/samples/sample5.aspx)]

Il *ShoppingCart.aspx* pagina include un **GridView** controllo denominato `CartList`. Questo controllo utilizza l'associazione di modelli per associare i dati del carrello acquisti dal database per il **GridView** controllo. Quando si imposta la `ItemType` proprietà del **GridView** controllare, l'espressione di associazione dati `Item` è disponibile nel markup del controllo e il controllo diventa fortemente tipizzato. Come accennato in precedenza in questa serie di esercitazioni, è possibile selezionare i dettagli del `Item` dell'oggetto usando IntelliSense. Per configurare un controllo dati per utilizzare l'associazione di modelli per selezionare i dati, è necessario impostare il `SelectMethod` proprietà del controllo. Nel markup riportato sopra, si imposta la `SelectMethod` usare il metodo GetShoppingCartItems che restituisce un elenco di `CartItem` oggetti. Il **GridView** controllo dati chiama il metodo momento adeguato del ciclo di vita della pagina e associa automaticamente i dati restituiti. Il `GetShoppingCartItems` metodo deve ancora essere aggiunto.

#### <a name="retrieving-the-shopping-cart-items"></a>Recupero di elementi del carrello acquisti

Si aggiungerà codice per il *ShoppingCart.aspx.cs* code-behind per recuperare e popolare il carrello acquisti dell'interfaccia utente.

1. Nelle **Esplora soluzioni**, fare doppio clic il *ShoppingCart.aspx* e quindi fare clic su **Visualizza codice**. Il *ShoppingCart.aspx.cs* file code-behind viene aperto nell'editor.
2. Sostituire il codice esistente con quello seguente:  

    [!code-csharp[Main](shopping-cart/samples/sample6.cs)]

Come indicato in precedenza, il `GridView` le chiamate di controllo dei dati di `GetShoppingCartItems` metodo nel momento adeguato del ciclo di vita della pagina del ciclo e associa automaticamente i dati restituiti. Il `GetShoppingCartItems` metodo crea un'istanza di `ShoppingCartActions` oggetto. Il codice Usa quindi tale istanza per restituire gli elementi nel carrello chiamando il `GetCartItems` (metodo).

### <a name="adding-products-to-the-shopping-cart"></a>Aggiungere prodotti al carrello

Quando entrambi i *ProductList. aspx* o nella *ProductDetails.aspx* viene visualizzata la pagina, l'utente sarà in grado di aggiungere il prodotto al carrello acquisti tramite un collegamento. Quando si fa clic sul collegamento, l'applicazione passa alla pagina elaborazione denominata *AddToCart.aspx*. Il *AddToCart.aspx* pagina chiamerà il `AddToCart` metodo nel `ShoppingCart` classe che è stato aggiunto in precedenza in questa esercitazione.

A questo punto, si aggiungerà un' **Aggiungi al carrello** collegamento a entrambe le *ProductList. aspx* pagina e il *ProductDetails.aspx* pagina. Questo collegamento includerà il prodotto `ID` che viene recuperato dal database.

1. Nelle **Esplora soluzioni**individuare e aprire la pagina dal titolo *ProductList. aspx*.
2. Aggiungere il markup evidenziato in giallo per il *ProductList. aspx* pagina in modo che l'intera pagina viene visualizzata come indicato di seguito:  

    [!code-aspx[Main](shopping-cart/samples/sample7.aspx?highlight=50-54)]

### <a name="testing-the-shopping-cart"></a>Il carrello acquisti di test

Eseguire l'applicazione per vedere come aggiungere prodotti al carrello acquisti.

1. Premere **F5** per eseguire l'applicazione.  
 Dopo che il progetto ricrea il database, il browser viene visualizzato il *default. aspx* pagina.
2. Selezionare **automobili** dal menu di navigazione di categoria.  
 Il *ProductList. aspx* viene visualizzata la pagina che mostra solo i prodotti inclusi nella categoria "Cars". 

    ![Il carrello della spesa - automobili](shopping-cart/_static/image5.png)
3. Scegliere il **Add to Cart** collegamento accanto al primo prodotto elencato (convertibili in auto).   
 Il *ShoppingCart.aspx* pagina viene visualizzata, la selezione visualizzata nel carrello. 

    ![Carrello - carrello della spesa](shopping-cart/_static/image6.png)
4. Visualizzare i prodotti aggiuntivi selezionando **piani** dal menu di navigazione di categoria.
5. Scegliere il **Add to Cart** collegamento accanto al primo prodotto elencato.  
 Il *ShoppingCart.aspx* page viene visualizzata con l'elemento aggiuntivo.
6. Chiudere il browser.

### <a name="calculating-and-displaying-the-order-total"></a>Calcolo e il totale dell'ordine di visualizzazione

Oltre ad aggiungere prodotti al carrello acquisti, si aggiungerà un `GetTotal` metodo di `ShoppingCart` classe e visualizzare l'importo totale dell'ordine nella pagina carrello acquisti.

1. In **Esplora soluzioni**, aprire il *ShoppingCartActions.cs* del file nei *logica* cartella.
2. Aggiungere il codice seguente `GetTotal` metodo evidenziato in giallo per il `ShoppingCart` classe, in modo che la classe viene visualizzata come indicato di seguito:   

    [!code-csharp[Main](shopping-cart/samples/sample8.cs?highlight=85-97)]

Prima di tutto la `GetTotal` metodo ottiene l'ID del carrello acquisti dell'utente. Quindi il metodo ottiene il carrello totale moltiplicando il prezzo del prodotto per la quantità del prodotto per ogni prodotto elencato nel carrello.

> [!NOTE] 
> 
> Il codice precedente Usa il tipo nullable "`int?`". Tipi nullable possono rappresentare tutti i valori di un tipo sottostante ma anche come un valore null. Per altre informazioni, vedere [utilizzando i tipi Nullable](https://msdn.microsoft.com/library/2cf62fcy(v=vs.110).aspx).


### <a name="modify-the-shopping-cart-display"></a>Modificare la visualizzazione del carrello acquisti

Successivamente si modificherà il codice per il *ShoppingCart.aspx* pagina per chiamare il `GetTotal` metodo e visualizzati in totale il *ShoppingCart.aspx* pagina durante il caricamento della pagina.

1. Nelle **Esplora soluzioni**, fare doppio clic il *ShoppingCart.aspx* pagina e selezionare **Visualizza codice**.
2. Nel *ShoppingCart.aspx.cs* del file, aggiornare il `Page_Load` gestore, aggiungendo il seguente codice evidenziato in giallo:   

    [!code-csharp[Main](shopping-cart/samples/sample9.cs?highlight=16-31)]

Quando la *ShoppingCart.aspx* pagina viene caricata, carica l'oggetto del carrello acquisti e quindi viene recuperato il totale del carrello chiamando il `GetTotal` metodo il `ShoppingCart` classe. Se il carrello acquisti è vuoto, viene visualizzato un messaggio in tal senso.

### <a name="testing-the-shopping-cart-total"></a>Verifica il totale del carrello

Eseguire ora l'applicazione per vedere come non è possibile solo aggiungere un prodotto al carrello, ma è possibile visualizzare il totale del carrello.

1. Premere **F5** per eseguire l'applicazione.  
 Verrà aperto il browser e visualizzare il *default. aspx* pagina.
2. Selezionare **automobili** dal menu di navigazione di categoria.
3. Scegliere il **Add To Cart** collegamento accanto al primo prodotto.   
 Il *ShoppingCart.aspx* page viene visualizzata con il totale degli ordini. 

    ![Il carrello della spesa - totale del carrello](shopping-cart/_static/image7.png)
4. Aggiungere ad altri prodotti (ad esempio, un piano) al carrello.
5. Il *ShoppingCart.aspx* viene visualizzata la pagina con un totale aggiornato per tutti i prodotti di cui è stato aggiunto. 

    ![Il carrello della spesa - più prodotti](shopping-cart/_static/image8.png)
6. Arrestare l'app in esecuzione, chiudere la finestra del browser.

### <a name="adding-update-and-checkout-buttons-to-the-shopping-cart"></a>Aggiunta di pulsanti di estrazione e Update al carrello acquisti

Per consentire agli utenti di modificare il carrello acquisti, si aggiungerà un' **Update** pulsante e una **Checkout** pulsante alla pagina carrello acquisti. Il **Checkout** pulsante non viene usato fino a quando non più avanti in questa serie di esercitazioni.

1. Nelle **Esplora soluzioni**, aprire il *ShoppingCart.aspx* pagina nella radice del progetto dell'applicazione web.
2. Per aggiungere il **Update** pulsante e il **Checkout** sul pulsante per il *ShoppingCart.aspx* pagina, aggiungere il markup evidenziato in giallo per i tag esistenti, come illustrato nel codice seguente:   

    [!code-aspx[Main](shopping-cart/samples/sample10.aspx?highlight=36-45)]

Quando l'utente sceglie il **Update** pulsante, il `UpdateBtn_Click` gestore eventi verrà chiamato. Questo gestore eventi chiama il codice che verrà aggiunto nel passaggio successivo.

Successivamente, è possibile aggiornare il codice contenuto nel *ShoppingCart.aspx.cs* file per riprodurre a ciclo di elementi del carrello e chiamare il `RemoveItem` e `UpdateItem` metodi.

1. Nelle **Esplora soluzioni**, aprire il *ShoppingCart.aspx.cs* file nella radice del progetto dell'applicazione web.
2. Aggiungere le seguenti sezioni di codice evidenziate in giallo per il *ShoppingCart.aspx.cs* file:   

    [!code-csharp[Main](shopping-cart/samples/sample11.cs?highlight=9-11,33,44-89)]

Quando l'utente fa clic il **Update** pulsante il *ShoppingCart.aspx* pagina, viene chiamato il metodo UpdateCartItems. Il metodo UpdateCartItems Ottiene i valori aggiornati per ogni elemento nel carrello acquisti. Chiama quindi il metodo UpdateCartItems il `UpdateShoppingCartDatabase` metodo (aggiunto e descritto nel passaggio successivo) per aggiungere o rimuovere gli articoli dal carrello acquisti. Una volta che il database è stato aggiornato per riflettere gli aggiornamenti al carrello acquisti, il **GridView** controllo viene aggiornato nella pagina carrello acquisti chiamando il `DataBind` metodo per il **GridView**. Inoltre, l'importo totale dell'ordine nella pagina carrello acquisti viene aggiornato per riflettere l'elenco aggiornato degli elementi.

### <a name="updating-and-removing-shopping-cart-items"></a>L'aggiornamento e rimozione di elementi del carrello acquisti

Nel *ShoppingCart.aspx* pagina, è possibile visualizzare i controlli sono stati aggiunti per la quantità di un elemento di aggiornamento e rimozione di un elemento. A questo punto, aggiungere il codice che userà questi controlli di funzionare.

1. In **Esplora soluzioni**, aprire il *ShoppingCartActions.cs* del file nei *logica* cartella.
2. Aggiungere il seguente codice evidenziato in giallo per il *ShoppingCartActions.cs* file di classe:   

    [!code-csharp[Main](shopping-cart/samples/sample12.cs?highlight=99-213)]

Il `UpdateShoppingCartDatabase` metodo, chiamato dal `UpdateCartItems` metodo sulle *ShoppingCart.aspx.cs* pagina, contiene la logica per aggiornare o rimuovere gli articoli dal carrello acquisti. Il `UpdateShoppingCartDatabase` metodo scorre tutte le righe all'interno dell'elenco di carrello degli acquisti. Se un articolo carrello acquisti è stato contrassegnato per essere rimosso o la quantità è minore di 1, il `RemoveItem` viene chiamato il metodo. In caso contrario, viene cercato il carrello acquisti quando aggiorna il `UpdateItem` viene chiamato il metodo. Dopo aver rimosso l'elemento di carrello degli acquisti o aggiornata, vengono salvate le modifiche del database.

Il `ShoppingCartUpdates` struttura viene usata per contenere tutti gli elementi del carrello acquisti. Il `UpdateShoppingCartDatabase` metodo viene utilizzato il `ShoppingCartUpdates` struttura per determinare se gli elementi necessari per essere aggiornato o rimosso.

Nella prossima esercitazione, si userà il `EmptyCart` metodo per cancellare il market carrello dopo l'acquisto di prodotti. Ma per ora, si useranno le `GetCount` metodo appena aggiunto per il *ShoppingCartActions.cs* file per determinare il numero di elementi presenti nel carrello acquisti.

### <a name="adding-a-shopping-cart-counter"></a>Aggiunta di un contatore di carrello degli acquisti

Per consentire all'utente di visualizzare il numero totale di elementi nel carrello acquisti, si aggiungerà un contatore per la *Site. master* pagina. Questo contatore verrà usato anche come un collegamento al carrello acquisti.

1. Nelle **Esplora soluzioni**, aprire il *Site. master* pagina.
2. Modificare il markup aggiungendo il collegamento di contatore carrello acquisti come mostrati in giallo per la sezione di navigazione in modo che venga visualizzato come segue:  

    [!code-html[Main](shopping-cart/samples/sample13.html?highlight=6)]
3. A questo punto, aggiornare il code-behind del *Site.Master.cs* file aggiungendo il codice evidenziato in giallo, come indicato di seguito:  

    [!code-csharp[Main](shopping-cart/samples/sample14.cs?highlight=11,77-84)]

Prima del rendering della pagina in formato HTML, il `Page_PreRender` viene generato l'evento. Nel `Page_PreRender` gestore, il conteggio totale del carrello acquisti viene determinato chiamando il `GetCount` (metodo). Il valore restituito viene aggiunto per il `cartCount` intervallo incluso nel markup del *Site. master* pagina. Il `<span>` tag consente gli elementi interni eseguire correttamente il rendering. Quando viene visualizzata una pagina del sito, verrà visualizzato il totale del carrello. L'utente può anche scegliere il totale del carrello per visualizzare il carrello acquisti.

## <a name="testing-the-completed-shopping-cart"></a>Il carrello acquisti completo di test

È possibile eseguire l'applicazione ora per vedere come è possibile aggiungere, delete e update elementi nel carrello acquisti. Il totale del carrello riporterà il costo totale di tutti gli elementi nel carrello acquisti.

1. Premere **F5** per eseguire l'applicazione.  
 Il browser si apre e Mostra le *default. aspx* pagina.
2. Selezionare **automobili** dal menu di navigazione di categoria.
3. Scegliere il **Add To Cart** collegamento accanto al primo prodotto.   
 Il *ShoppingCart.aspx* page viene visualizzata con il totale degli ordini.
4. Selezionare **piani** dal menu di navigazione di categoria.
5. Scegliere il **Add To Cart** collegamento accanto al primo prodotto.
6. Impostare la quantità del primo elemento nel carrello acquisti su 3 e selezionare il **Rimuovi elemento** casella di controllo del secondo elemento.<a id="a"></a>
7. Scegliere il **aggiornare** pulsante per aggiornare la pagina carrello degli acquisti e visualizzare il nuovo totale dell'ordine. 

    ![Carrello acquisti - aggiornamento del carrello](shopping-cart/_static/image9.png)

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato creato un carrello della spesa per l'applicazione di esempio Wingtip Toys Web Form. Durante questa esercitazione è stato usato Entity Framework Code First, le annotazioni dei dati, controlli dati fortemente tipizzati e associazione di modelli.

Il carrello acquisti supporta l'aggiunta, eliminazione e aggiornamento degli elementi che l'utente ha selezionato per l'acquisto. Oltre all'implementazione della funzionalità di carrello degli acquisti, si è appreso come visualizzare elementi del carrello acquisti in un **GridView** controllano e calcolare il totale degli ordini.

## <a name="addition-information"></a>Informazioni aggiuntive

[Cenni preliminari sullo stato della sessione ASP.NET](https://msdn.microsoft.com/library/ms178581.aspx)

> [!div class="step-by-step"]
> [Precedente](display_data_items_and_details.md)
> [Successivo](checkout-and-payment-with-paypal.md)
