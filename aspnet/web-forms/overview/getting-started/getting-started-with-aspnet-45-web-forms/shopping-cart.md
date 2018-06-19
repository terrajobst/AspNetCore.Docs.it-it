---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
title: Carrello degli acquisti | Documenti Microsoft
author: Erikre
description: Questa serie di esercitazioni verranno fornite le nozioni di base della creazione di un'applicazione Web Form ASP.NET tramite ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per abbiamo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 6898c601-6c31-432f-8388-e6843f8a17cb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/shopping-cart
msc.type: authoredcontent
ms.openlocfilehash: a8e96da7737cdf649575711a464c4f7726cb6ded
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890720"
---
<a name="shopping-cart"></a>Carrello acquisti
====================
da [Erik Reitan](https://github.com/Erikre)

[Scarica progetto di esempio Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [scaricare E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Questa serie di esercitazioni verranno fornite le nozioni di base della creazione di un'applicazione Web Form ASP.NET utilizzando ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per Web. Un Visual Studio 2013 [progetto con il codice sorgente c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) complemento questa serie di esercitazioni è disponibile.


Questa esercitazione vengono descritti la logica di business richiesta per aggiungere un carrello acquisti per l'applicazione Web Form ASP.NET di esempio Wingtip Toys. In questa esercitazione si basa sull'esercitazione precedente "Visualizzazione elementi e dettagli dati" e fa parte della serie di esercitazioni Wingtip giocattoli archivio. Dopo aver completato questa esercitazione, gli utenti della propria applicazione di esempio sarà in grado di aggiungere, rimuovere e modificare i prodotti nel carrello acquisti.

## <a name="what-youll-learn"></a>Illustra quanto segue:

1. Come creare un carrello acquisti per l'applicazione web.
2. Come consentire agli utenti di aggiungere elementi al carrello.
3. Come aggiungere un [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview(v=vs.110).aspx#introduction) controllo per visualizzare i dettagli del carrello acquisti.
4. Come calcolare e visualizzare il totale dell'ordine.
5. Come rimuovere e aggiornare gli elementi nel carrello acquisti.
6. Come includere un contatore di carrello acquisti.

## <a name="code-features-in-this-tutorial"></a>Funzionalità di codice in questa esercitazione:

1. Entity Framework Code First
2. Annotazioni dei dati
3. Fortemente tipizzate controlli dati
4. Associazione di modelli

## <a name="creating-a-shopping-cart"></a>Creazione di un carrello acquisti

In precedenza in questa serie di esercitazioni, è stato aggiunto pagine e il codice per visualizzare i dati di prodotto da un database. In questa esercitazione si creerà un carrello acquisti per gestire i prodotti che gli utenti sono interessati all'acquisto. Gli utenti saranno in grado di cercare e aggiungere elementi al carrello, anche se non sono registrati o registrati. Per gestire l'accesso di carrello acquisti, si assegnano utenti univoco `ID` utilizzando un identificatore univoco globale (GUID) quando l'utente accede il market carrello per la prima volta. Archiviare questo `ID` utilizzando lo stato della sessione ASP.NET.

> [!NOTE] 
> 
> Lo stato della sessione ASP.NET è una posizione comoda in cui archiviare informazioni specifiche dell'utente che scadranno dopo che l'utente ha lasciato il sito. Durante l'utilizzo improprio dello stato sessione può avere implicazioni sulle prestazioni siti di grandi dimensioni, chiaro l'utilizzo della sessione stato funziona anche per scopi dimostrativi. Il progetto di esempio Wingtip Toys viene illustrato come utilizzare lo stato della sessione senza un provider esterno, in cui lo stato della sessione è archiviato in-process nel server web che ospita il sito. Per i siti di grandi dimensioni che forniscono più istanze di un'applicazione o per i siti che eseguono più istanze di un'applicazione in server diversi, è possibile utilizzare **servizio Cache di Windows Azure**. Il servizio Cache fornisce un servizio di memorizzazione nella cache distribuito che è esterno al sito web e risolve il problema dello stato sessione in-process. Per ulteriori informazioni, vedere [come utilizzare lo stato della sessione ASP.NET con siti Web di Azure](https://docs.microsoft.com/azure/redis-cache/cache-aspnet-session-state-provider).


### <a name="add-cartitem-as-a-model-class"></a>Aggiungere CartItem come una classe di modello

Più indietro in questa serie di esercitazioni, è definito lo schema per i dati di categoria e prodotto tramite la creazione di `Category` e `Product` classi nel *modelli* cartella. A questo punto, aggiungere una nuova classe per definire lo schema per il carrello acquisti. Più avanti in questa esercitazione si aggiungerà una classe per gestire l'accesso ai dati di `CartItem` tabella. Questa classe fornirà la logica di business per aggiungere, rimuovere e aggiornare gli elementi nel carrello acquisti.

1. Fare doppio clic su di *modelli* cartella e selezionare **Aggiungi**  - &gt; **nuovo elemento**. 

    ![Carrello acquisti - nuovo elemento](shopping-cart/_static/image1.png)
2. Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**. Selezionare **codice**, quindi selezionare **classe**. 

    ![Shopping Cart - Aggiungi finestra di dialogo Nuovo elemento](shopping-cart/_static/image2.png)
3. Denominare la nuova classe *CartItem.cs*.
4. Fare clic su **Aggiungi**.  
   Il nuovo file di classe viene visualizzato nell'editor.
5. Sostituire il codice predefinito con il codice seguente:   

    [!code-csharp[Main](shopping-cart/samples/sample1.cs)]

La `CartItem` classe contiene lo schema che definisce ogni prodotto in cui un utente aggiunge al carrello. Questa classe è simile alle altre classi di schema creato in precedenza in questa serie di esercitazioni. Per convenzione, Code First di Entity Framework si aspetta che la chiave primaria per la `CartItem` tabella può essere `CartItemId` o `ID`. Tuttavia, il codice esegue l'override del comportamento predefinito utilizzando l'annotazione dei dati `[Key]` attributo. Il `Key` attributo della proprietà ItemId specifica che il `ItemID` proprietà è la chiave primaria.

Il `CartId` proprietà specifica il `ID` dell'utente che è associato alla voce per l'acquisto. Verrà aggiunto il codice per creare l'utente `ID` quando l'utente accede il carrello acquisti. Questo `ID` verrà anche archiviata come una variabile di sessione ASP.NET.

### <a name="update-the-product-context"></a>Aggiornare il contesto del prodotto

Oltre ad aggiungere il `CartItem` (classe), è necessario aggiornare la classe di contesto di database che gestisce le classi di entità e che fornisce accesso ai dati nel database. A tale scopo, si aggiungeranno appena creato `CartItem` del modello di classe per la `ProductContext` classe.

1. In **Esplora**, trovare e aprire il *ProductContext.cs* file nel *modelli* cartella.
2. Aggiungere il codice evidenziato per il *ProductContext.cs* file come segue:  

    [!code-csharp[Main](shopping-cart/samples/sample2.cs?highlight=14)]

Come accennato in precedenza in questa serie di esercitazioni, il codice nel *ProductContext.cs* file aggiunge il `System.Data.Entity` dello spazio dei nomi in modo da poter accedere a tutte le funzionalità principali di Entity Framework. Questa funzionalità include la possibilità di eseguire una query, inserimento, aggiornamento ed eliminare i dati quando si lavora con oggetti fortemente tipizzati. Il `ProductContext` classe aggiunge accesso appena aggiunto `CartItem` classe del modello.

### <a name="managing-the-shopping-cart-business-logic"></a>Gestire la logica di Business carrello acquisti

Successivamente, si creerà il `ShoppingCart` classe in un nuovo *logica* cartella. Il `ShoppingCart` classe gestisce l'accesso ai dati di `CartItem` tabella. La classe includerà inoltre la logica di business per aggiungere, rimuovere e aggiornare gli elementi nel carrello acquisti.

La logica di carrello degli acquisti che verranno aggiunti conterrà la funzionalità per gestire le azioni seguenti:

1. Aggiunta di elementi al carrello acquisti
2. Rimozione di elementi dal carrello
3. Ottenere l'ID del carrello acquisti
4. Recupero elementi dal carrello
5. Per un totale la quantità di tutti gli elementi di carrello acquisti
6. Aggiornamento dei dati del carrello acquisti

Una pagina carrello acquisti (*ShoppingCart.aspx*) e la classe di carrello essere usata insieme per accedere ai dati del carrello acquisti. La pagina carrello degli acquisti visualizzerà tutti gli elementi da cui che l'utente aggiunge al carrello. Oltre il carrello pagina e la classe, si creerà una pagina (*AddToCart.aspx*) per aggiungere il carrello acquisti di prodotti. Si aggiungerà inoltre codice per il *ProductList* pagina e *ProductDetails.aspx* pagina che fornirà un collegamento per il *AddToCart.aspx* pagina, in modo che l'utente può aggiungere prodotti per il carrello acquisti.

Nel diagramma seguente viene illustrato il processo di base che si verifica quando l'utente aggiunge un prodotto per il carrello acquisti.

![Carrello acquisti - aggiunta al carrello](shopping-cart/_static/image3.png)

Quando l'utente fa clic il **Add To Cart** collegamento in entrambi i *ProductList* pagina o *ProductDetails.aspx* pagina, l'applicazione verrà visualizzata la *AddToCart.aspx* pagina e quindi automaticamente al *ShoppingCart.aspx* pagina. Il *AddToCart.aspx* pagina aggiungerà il prodotto seleziona al carrello chiamando un metodo nella classe ShoppingCart. Il *ShoppingCart.aspx* pagina verrà visualizzati i prodotti che sono stati aggiunti al carrello.

#### <a name="creating-the-shopping-cart-class"></a>Creazione della classe di carrello acquisti

La `ShoppingCart` classe verrà aggiunta in una cartella separata nell'applicazione in modo che non esiste una chiara distinzione tra il modello (cartella Models), le pagine (cartella radice) e la logica (cartella logica).

1. In **Esplora**, fare doppio clic su di **WingtipToys**del progetto e selezionare **Aggiungi**-&gt;**nuova cartella**. Denominare la nuova cartella *logica*.
2. Fare doppio clic su di *logica* cartella e quindi selezionare **Aggiungi**  - &gt; **nuovo elemento**.
3. Aggiungere un nuovo file di classe denominato *ShoppingCartActions.cs*.
4. Sostituire il codice predefinito con il codice seguente:   

    [!code-csharp[Main](shopping-cart/samples/sample3.cs)]

Il `AddToCart` metodo consente di singoli prodotti da includere nel carrello acquisti in base al prodotto `ID`. Il prodotto viene aggiunto al carrello, o se il carrello contiene già un elemento per il prodotto, la quantità viene incrementata.

Il `GetCartId` il metodo restituisce il carrello `ID` per l'utente. Il carrello `ID` viene utilizzato per tenere traccia degli elementi di un utente nel carrello acquisti. Se l'utente non dispone di un carrello esistente `ID`, un carrello nuovo `ID` viene creata per loro. Se l'utente ha effettuato l'accesso come utente registrato, il carrello `ID` è impostato su nome utente. Tuttavia, se l'utente non è connesso, il carrello `ID` è impostata su un valore univoco (GUID). Un GUID garantisce che solo un carrello viene creato per ogni utente, in base a una sessione.

Il `GetCartItems` metodo restituisce un elenco di shopping cart elementi per l'utente. Più avanti in questa esercitazione, si noterà che l'associazione di modelli viene utilizzata per visualizzare gli elementi del carrello nel carrello acquisti tramite il `GetCartItems` metodo.

### <a name="creating-the-add-to-cart-functionality"></a>Creare la funzionalità Aggiungi al carrello

Come accennato in precedenza, si creerà una pagina di elaborazione denominata *AddToCart.aspx* che verrà usato per aggiungere nuovi prodotti al carrello acquisti dell'utente. Questa pagina verrà chiamato il `AddToCart` metodo la `ShoppingCart` classe appena creata. Il *AddToCart.aspx* pagina si aspetta che un prodotto `ID` viene passato al metodo. Questo prodotto `ID` verrà utilizzato quando si chiama il `AddToCart` metodo la `ShoppingCart` classe.

> [!NOTE] 
> 
> È necessario modificare il code-behind (*AddToCart.aspx.cs*) per questa pagina, non la pagina dell'interfaccia utente (*AddToCart.aspx*).


#### <a name="to-create-the-add-to-cart-functionality"></a>Per creare Add To Cart funzionalità:

1. In **Esplora**, fare doppio clic su di **WingtipToys**del progetto, fare clic su **Aggiungi**  - &gt; **nuovo elemento**.  
   Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Aggiungere una nuova pagina standard (Web Form) per l'applicazione denominata *AddToCart.aspx*. 

    ![Shopping Cart - aggiungere Web Form](shopping-cart/_static/image4.png)
3. In **Esplora**, fare doppio clic su di *AddToCart.aspx* pagina e quindi fare clic su **Visualizza codice**. Il *AddToCart.aspx.cs* file code-behind è aperto nell'editor.
4. Sostituire il codice esistente nel *AddToCart.aspx.cs* codice con quanto segue:   

    [!code-csharp[Main](shopping-cart/samples/sample4.cs)]

Quando il *AddToCart.aspx* pagina viene caricata, il prodotto `ID` viene recuperato dalla stringa di query. Successivamente, un'istanza della classe di carrello acquisti viene creata e utilizzata per chiamare il `AddToCart` metodo aggiunto precedentemente in questa esercitazione. Il `AddToCart` contenuta nel metodo il *ShoppingCartActions.cs* file, che include la logica per aggiungere il prodotto selezionato per il carrello acquisti o aumentare la quantità del prodotto del prodotto selezionato. Se il prodotto è stato aggiunto al carrello acquisti, il prodotto viene aggiunto per il `CartItem` tabella del database. Se il prodotto è già stata aggiunta al carrello e l'utente aggiunge un elemento aggiuntivo dello stesso prodotto, la quantità di prodotto viene incrementata il `CartItem` tabella. Infine, la pagina reindirizza il *ShoppingCart.aspx* pagina che verrà aggiunto nel passaggio successivo, in cui l'utente visualizza un elenco aggiornato di articoli nel carrello.

Come menzionato in precedenza, un utente `ID` viene utilizzato per identificare i prodotti che sono associati a un utente specifico. Questo `ID` viene aggiunto a una riga di `CartItem` tabella ogni volta che l'utente aggiunge un prodotto per il carrello acquisti.

### <a name="creating-the-shopping-cart-ui"></a>Creazione del carrello degli acquisti dell'interfaccia utente

Il *ShoppingCart.aspx* pagina verrà visualizzati i prodotti che l'utente ha aggiunto al carrello acquisti. Fornirà anche la possibilità di aggiungere, rimuovere e aggiornare gli elementi nel carrello acquisti.

1. In **Esplora**, fare doppio clic su **WingtipToys**, fare clic su **Aggiungi**  - &gt; **nuovo elemento**.  
   Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Aggiungere una nuova pagina (Web Form) che include una pagina master selezionando **Web Form mediante pagina Master**. Denominare la nuova pagina *ShoppingCart.aspx*.
3. Selezionare **Site. master** per collegare la pagina master per l'oggetto appena creato *aspx* pagina.
4. Nel *ShoppingCart.aspx* pagina, sostituire il codice esistente con il markup seguente:   

    [!code-aspx[Main](shopping-cart/samples/sample5.aspx)]

Il *ShoppingCart.aspx* pagina include un **GridView** controllo denominato `CartList`. Questo controllo utilizza l'associazione di modelli per associare i dati del carrello acquisti del database per il **GridView** controllo. Quando si imposta la `ItemType` proprietà del **GridView** di controllo, espressione di associazione dati `Item` è disponibile nel markup del controllo e il controllo diventa fortemente tipizzato. Come accennato in precedenza in questa serie di esercitazioni, è possibile selezionare i dettagli del `Item` utilizzando IntelliSense. Per configurare un controllo di dati per l'utilizzo di associazione di modelli per selezionare i dati, impostare il `SelectMethod` proprietà del controllo. Nel codice precedente, si imposta la `SelectMethod` per utilizzare il metodo GetShoppingCartItems che restituisce un elenco di `CartItem` oggetti. Il **GridView** controllo dati chiama il metodo nel momento adeguato del ciclo di vita della pagina e associa automaticamente i dati restituiti. Il `GetShoppingCartItems` metodo deve ancora essere aggiunto.

#### <a name="retrieving-the-shopping-cart-items"></a>Recuperare gli elementi del carrello acquisti

Aggiungere quindi codice per il *ShoppingCart.aspx.cs* code-behind per recuperare e popolare il carrello acquisti dell'interfaccia utente.

1. In **Esplora**, fare doppio clic su di *ShoppingCart.aspx* pagina e quindi fare clic su **Visualizza codice**. Il *ShoppingCart.aspx.cs* file code-behind è aperto nell'editor.
2. Sostituire il codice esistente con quello seguente:  

    [!code-csharp[Main](shopping-cart/samples/sample6.cs)]

Come indicato in precedenza, il `GridView` chiamate di controllo ai dati di `GetShoppingCartItems` metodo nel momento adeguato del ciclo di vita della pagina del ciclo e associa automaticamente i dati restituiti. Il `GetShoppingCartItems` metodo crea un'istanza di `ShoppingCartActions` oggetto. Quindi, il codice usa tale istanza per restituire gli elementi nel carrello chiamando il `GetCartItems` metodo.

### <a name="adding-products-to-the-shopping-cart"></a>Aggiunta di prodotti al carrello

Quando entrambi i *ProductList.aspx* o *ProductDetails.aspx* viene visualizzata la pagina, l'utente sarà in grado di aggiungere il prodotto al carrello utilizzando un collegamento. Quando si fa clic sul collegamento, l'applicazione passa alla pagina elaborazione denominata *AddToCart.aspx*. Il *AddToCart.aspx* pagina chiamerà il `AddToCart` metodo la `ShoppingCart` classe che è stato aggiunto in precedenza in questa esercitazione.

A questo punto, si aggiungerà un **Aggiungi al carrello** collegamento sia il *ProductList.aspx* pagina e *ProductDetails.aspx* pagina. Questo collegamento verrà includono il prodotto `ID` recuperati dal database.

1. In **Esplora**, trovare e aprire la pagina denominata *ProductList.aspx*.
2. Aggiungere il markup evidenziato in giallo per il *ProductList.aspx* pagina in modo che l'intera pagina viene visualizzata come indicato di seguito:  

    [!code-aspx[Main](shopping-cart/samples/sample7.aspx?highlight=50-54)]

### <a name="testing-the-shopping-cart"></a>Test del carrello degli acquisti

Eseguire l'applicazione per vedere come aggiungere prodotti al carrello acquisti.

1. Premere **F5** per eseguire l'applicazione.  
 Dopo che il progetto ricrea il database, aprire il browser e visualizzare il *Default.aspx* pagina.
2. Selezionare **automobili** dal menu di navigazione di categoria.  
 Il *ProductList.aspx* viene visualizzata la pagina che mostra solo i prodotti inclusi nella categoria "Auto". 

    ![Carrello acquisti - automobili](shopping-cart/_static/image5.png)
3. Fare clic su di **Aggiungi al carrello** collegamento accanto a prima del prodotto elencato (car convertibile).   
 Il *ShoppingCart.aspx* verrà visualizzata la pagina, che mostra la selezione nel carrello. 

    ![Carrello acquisti - carrello](shopping-cart/_static/image6.png)
4. Visualizzare i prodotti aggiuntivi selezionando **piani** dal menu di navigazione di categoria.
5. Fare clic su di **Aggiungi al carrello** collegamento accanto a elencato prima del prodotto.  
 Il *ShoppingCart.aspx* verrà visualizzata la pagina con l'elemento aggiuntiva.
6. Chiudere il browser.

### <a name="calculating-and-displaying-the-order-total"></a>Calcolo e il totale dell'ordine di visualizzazione

Oltre ad aggiungere prodotti al carrello acquisti, si aggiungerà un `GetTotal` metodo per la `ShoppingCart` classe e visualizzare la quantità totale dell'ordine nella pagina carrello acquisti.

1. In **Esplora**, aprire il *ShoppingCartActions.cs* file nel *logica* cartella.
2. Aggiungere il seguente `GetTotal` metodo evidenziato in giallo per la `ShoppingCart` classe, in modo che la classe viene visualizzata come indicato di seguito:   

    [!code-csharp[Main](shopping-cart/samples/sample8.cs?highlight=85-97)]

Prima di tutto, il `GetTotal` metodo ottiene l'ID del carrello acquisti dell'utente. Il metodo recupera quindi il carrello totale moltiplicando il prezzo del prodotto per la quantità del prodotto per ogni prodotto elencato nel carrello.

> [!NOTE] 
> 
> Il codice precedente utilizza il tipo nullable "`int?`". Tipi nullable possono rappresentare tutti i valori di un tipo sottostante ma anche come un valore null. Per ulteriori informazioni, vedere [utilizzando i tipi Nullable](https://msdn.microsoft.com/library/2cf62fcy(v=vs.110).aspx).


### <a name="modify-the-shopping-cart-display"></a>Modificare la visualizzazione del carrello acquisti

Successivamente si verrà modificato il codice per il *ShoppingCart.aspx* pagina per chiamare il `GetTotal` (metodo) e visualizzare un totale nel *ShoppingCart.aspx* pagina durante il caricamento della pagina.

1. In **Esplora**, fare doppio clic su di *ShoppingCart.aspx* pagina e selezionare **Visualizza codice**.
2. Nel *ShoppingCart.aspx.cs* file, aggiornare il `Page_Load` gestore aggiungendo il seguente codice evidenziato in giallo:   

    [!code-csharp[Main](shopping-cart/samples/sample9.cs?highlight=16-31)]

Quando il *ShoppingCart.aspx* caricamento della pagina, carica l'oggetto del carrello acquisti e recupera quindi il totale di carrello acquisti chiamando il `GetTotal` metodo la `ShoppingCart` classe. Se il carrello acquisti è vuoto, viene visualizzato un messaggio in tal senso.

### <a name="testing-the-shopping-cart-total"></a>Test totale carrello acquisti

Eseguire l'applicazione per vedere come non è possibile solo aggiungere un prodotto per il carrello acquisti, ma è possibile visualizzare il totale di carrello acquisti.

1. Premere **F5** per eseguire l'applicazione.  
 Aprire il browser e visualizzare il *Default.aspx* pagina.
2. Selezionare **automobili** dal menu di navigazione di categoria.
3. Fare clic su di **Add To Cart** collegamento accanto a prima del prodotto.   
 Il *ShoppingCart.aspx* viene visualizzata la pagina con il totale dell'ordine. 

    ![Shopping Cart - totale carrello](shopping-cart/_static/image7.png)
4. Aggiungere alcuni altri prodotti (ad esempio, un piano) al carrello.
5. Il *ShoppingCart.aspx* viene visualizzata la pagina con un totale per tutti i prodotti aggiunti aggiornato. 

    ![Carrello acquisti - più prodotti](shopping-cart/_static/image8.png)
6. Arrestare l'applicazione in esecuzione, chiudere la finestra del browser.

### <a name="adding-update-and-checkout-buttons-to-the-shopping-cart"></a>Aggiunta di pulsanti di estrazione e di aggiornamento al carrello

Per consentire agli utenti di modificare il carrello acquisti, aggiungerai un **aggiornamento** pulsante e un **estrazione** pulsante alla pagina carrello acquisti. Il **estrazione** pulsante non viene utilizzato fino a quando non più avanti in questa serie di esercitazioni.

1. In **Esplora**, aprire il *ShoppingCart.aspx* pagina nella radice del progetto di applicazione web.
2. Per aggiungere il **aggiornamento** pulsante e **estrazione** pulsante per il *ShoppingCart.aspx* pagina, aggiungere il markup evidenziato in giallo al codice esistente, come illustrato di codice seguente:   

    [!code-aspx[Main](shopping-cart/samples/sample10.aspx?highlight=36-45)]

Quando l'utente sceglie il **aggiornamento** pulsante, il `UpdateBtn_Click` gestore verrà chiamato. Questo gestore eventi chiama il codice che verrà aggiunto nel passaggio successivo.

Successivamente, è possibile aggiornare il codice contenuto nel *ShoppingCart.aspx.cs* file per scorrere gli elementi del carrello e chiamata di `RemoveItem` e `UpdateItem` metodi.

1. In **Esplora**, aprire il *ShoppingCart.aspx.cs* file nella radice del progetto di applicazione web.
2. Aggiungere le seguenti sezioni di codice evidenziate in giallo per il *ShoppingCart.aspx.cs* file:   

    [!code-csharp[Main](shopping-cart/samples/sample11.cs?highlight=9-11,33,44-89)]

Quando l'utente fa clic il **aggiornamento** pulsante il *ShoppingCart.aspx* pagina, viene chiamato il metodo UpdateCartItems. Il metodo UpdateCartItems Ottiene i valori aggiornati per ogni elemento nel carrello acquisti. Chiama quindi il metodo UpdateCartItems il `UpdateShoppingCartDatabase` metodo (aggiunto e descritto nel passaggio successivo) per aggiungere o rimuovere gli elementi dal carrello. Una volta che il database è stato aggiornato per riflettere gli aggiornamenti al carrello acquisti, il **GridView** controllo viene aggiornato nella pagina carrello acquisti chiamando il `DataBind` metodo per il **GridView**. Inoltre, la quantità totale dell'ordine della pagina carrello acquisti viene aggiornata per riflettere l'elenco aggiornato degli elementi.

### <a name="updating-and-removing-shopping-cart-items"></a>L'aggiornamento e rimozione di articoli del carrello acquisti

Nel *ShoppingCart.aspx* pagina, è possibile visualizzare i controlli sono stati aggiunti per la quantità di un elemento di aggiornamento e rimozione di un elemento. A questo punto, aggiungere il codice che consentono a questi controlli di lavoro.

1. In **Esplora**, aprire il *ShoppingCartActions.cs* file nel *logica* cartella.
2. Aggiungere il seguente codice evidenziato in giallo per il *ShoppingCartActions.cs* file di classe:   

    [!code-csharp[Main](shopping-cart/samples/sample12.cs?highlight=99-213)]

Il `UpdateShoppingCartDatabase` chiamato dal metodo di `UpdateCartItems` metodo sul *ShoppingCart.aspx.cs* pagina, contiene la logica per aggiornare o rimuovere gli elementi dal carrello. Il `UpdateShoppingCartDatabase` metodo scorre tutte le righe all'interno dell'elenco di carrello acquisti. Se un elemento del carrello acquisti è stato contrassegnato per essere rimosso o la quantità è inferiore a quello di `RemoveItem` metodo viene chiamato. In caso contrario, l'elemento carrello acquisti è selezionata per gli aggiornamenti quando il `UpdateItem` metodo viene chiamato. Dopo aver rimosso l'elemento carrello acquisti o aggiornata, vengono salvate le modifiche del database.

Il `ShoppingCartUpdates` struttura viene usata per contenere tutti gli elementi del carrello acquisti. Il `UpdateShoppingCartDatabase` metodo utilizza il `ShoppingCartUpdates` struttura per determinare se uno degli elementi devono essere aggiornato o rimosso.

Nella prossima esercitazione, si utilizzerà il `EmptyCart` metodo per cancellare il market carrello dopo l'acquisto di prodotti. Ma per ora, si utilizzerà il `GetCount` metodo appena aggiunto per il *ShoppingCartActions.cs* file per determinare il numero di elementi nel carrello acquisti.

### <a name="adding-a-shopping-cart-counter"></a>Aggiunta di un contatore di carrello acquisti

Per consentire all'utente di visualizzare il numero totale di elementi nel carrello acquisti, si aggiungerà un contatore per la *Site. master* pagina. Questo contatore fungerà anche un collegamento al carrello.

1. In **Esplora**, aprire il *Site. master* pagina.
2. Modificare il markup aggiungendo il collegamento al contatore carrello, come illustrato in giallo nella sezione di navigazione viene visualizzato come indicato di seguito:  

    [!code-html[Main](shopping-cart/samples/sample13.html?highlight=6)]
3. Aggiornare quindi il code-behind del *Site.Master.cs* file aggiungendo il codice evidenziato in giallo, come indicato di seguito:  

    [!code-csharp[Main](shopping-cart/samples/sample14.cs?highlight=11,77-84)]

Prima del rendering della pagina in formato HTML, il `Page_PreRender` viene generato l'evento. Nel `Page_PreRender` gestore, il conteggio totale del carrello acquisti viene determinato chiamando il `GetCount` metodo. Il valore restituito viene aggiunto per il `cartCount` incluso nel markup di intervallo il *Site. master* pagina. Il `<span>` tag consente gli elementi interni eseguire correttamente il rendering. Quando viene visualizzata una pagina del sito, verrà visualizzato il totale di carrello acquisti. L'utente è anche possibile scegliere il totale di carrello degli acquisti per visualizzare il carrello acquisti.

## <a name="testing-the-completed-shopping-cart"></a>Test completato carrello

È possibile eseguire l'applicazione per vedere come è possibile aggiungere, delete e update elementi nel carrello acquisti. Il totale di carrello acquisti rifletterà il costo totale di tutti gli elementi nel carrello acquisti.

1. Premere **F5** per eseguire l'applicazione.  
 Il browser viene visualizzata e viene illustrato il *Default.aspx* pagina.
2. Selezionare **automobili** dal menu di navigazione di categoria.
3. Fare clic su di **Add To Cart** collegamento accanto a prima del prodotto.   
 Il *ShoppingCart.aspx* viene visualizzata la pagina con il totale dell'ordine.
4. Selezionare **piani** dal menu di navigazione di categoria.
5. Fare clic su di **Add To Cart** collegamento accanto a prima del prodotto.
6. Impostare la quantità del primo elemento nel carrello acquisti su 3 e selezionare il **Rimuovi elemento** casella di controllo del secondo elemento.<a id="a"></a>
7. Fare clic su di **aggiornare** pulsante per aggiornare la pagina carrello degli acquisti e visualizzare il nuovo totale dell'ordine. 

    ![Carrello acquisti - aggiornamento del carrello](shopping-cart/_static/image9.png)

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato creato un carrello acquisti dell'applicazione di esempio Wingtip Toys Web Form. Durante questa esercitazione è stato utilizzato Entity Framework Code First, le annotazioni dei dati, i controlli di dati fortemente tipizzati e associazione del modello.

Il carrello acquisti supporta l'aggiunta, eliminazione e aggiornamento degli elementi che l'utente ha selezionato per l'acquisto. Oltre all'implementazione della funzionalità di carrello acquisti, si è appreso come visualizzare gli elementi di carrello acquisti in un **GridView** controllare e calcolare il totale dell'ordine.

## <a name="addition-information"></a>Informazioni aggiuntive

[Panoramica dello stato della sessione ASP.NET](https://msdn.microsoft.com/library/ms178581.aspx)

> [!div class="step-by-step"]
> [Precedente](display_data_items_and_details.md)
> [Successivo](checkout-and-payment-with-paypal.md)
