---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Parte 8: Carrello acquisti con gli aggiornamenti Ajax | Documenti Microsoft'
author: jongalloway
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio. Parte 8 copre un carrello acquisti con gli aggiornamenti Ajax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 195c01ff0d71b2bfd0c00e71244d47a166330921
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="part-8-shopping-cart-with-ajax-updates"></a>Parte 8: Shopping Cart con gli aggiornamenti Ajax
====================
da [Jon Galloway](https://github.com/jongalloway)

> L'archivio di musica MVC è un'applicazione di esercitazione che vengono presentati e dettagliate sull'utilizzo di MVC ASP.NET e Visual Studio per lo sviluppo web.  
>   
> L'archivio di musica MVC è un'implementazione dell'archivio di esempio semplice che vende album musicali online e ne implementa amministrazione sito di base, account utente e funzionalità di carrello acquisti.  
>   
> Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio. Parte 8 copre un carrello acquisti con gli aggiornamenti Ajax.


Faremo in modo agli utenti di effettuare album nel carrello senza registrazione, ma sarà necessario registrare come guest a completare il checkpoint. Il processo di acquisto e l'estrazione verrà separato in due controller: un ShoppingCart Controller che consente l'aggiunta di elementi in modo anonimo a un carrello e un Controller di estrazione che gestisce il processo di estrazione. Si sarà iniziare con il carrello in questa sezione, quindi compilare il processo di estrazione nella sezione seguente.

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a>Aggiunta di classi di modello carrello, ordine e OrderDetail

I processi del carrello e l'estrazione renderà utilizzare di alcune nuove classi. Fare clic sulla cartella di modelli e aggiungere una classe di carrello (Cart.cs) con il codice seguente.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

Questa classe è molto simile ad altri utenti che è stata utilizzata fino a questo punto, fatta eccezione per l'attributo [Key] per la proprietà RecordId. L'elemento di carrello disporrà di un identificatore di stringa denominato CartID per consentire di acquisti anonimi, ma la tabella include una chiave primaria di tipo integer denominata RecordId. Per convenzione, Code-First di Entity Framework prevede che la chiave primaria per una tabella denominata carrello sarà CartId o ID, ma è possibile eseguire facilmente l'override che tramite le annotazioni o codice se lo si desidera. Questo è un esempio di come è possibile utilizzare le convenzioni semplice in Entity Framework Code-First quando sono in base alle us, ma ci non stiamo è vincolati da li quando questo non avviene.

Successivamente, aggiungere una classe Order (cs) con il codice seguente.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

Questa classe tiene traccia delle informazioni di riepilogo e il recapito di un ordine. **Non verrà compilato ancora**perché non è una proprietà di navigazione OrderDetails che dipende da una classe è non ancora creato. Si correggerà che ora aggiungendo una classe denominata OrderDetail.cs, aggiungendo il codice seguente.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

Ultimo aggiornamento da apportare alla classe MusicStoreEntities includere DbSets che espongono le nuove classi di modello, incluso anche un elemento DbSet&lt;artista&gt;. La classe MusicStoreEntities aggiornata viene visualizzato come di seguito.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a>Gestire la logica di business Shopping Cart

Successivamente, si creerà la classe ShoppingCart nella cartella Models. Il modello ShoppingCart gestisce l'accesso ai dati alla tabella carrello. Inoltre, gestirà la logica di business per l'aggiunta e rimozione di elementi dal carrello.

Poiché non si desidera richiedere agli utenti di iscriversi a un account solo aggiungere elementi al carrello acquisti, si assegnerà agli utenti un identificatore univoco temporaneo (utilizzando un GUID o un identificatore univoco globale) quando accedono il carrello acquisti. Questo ID utilizzando la classe di sessione ASP.NET verranno archiviate.

*Nota: La sessione ASP.NET è una posizione comoda in cui archiviare informazioni specifiche dell'utente che scadranno dopo che gli utenti lasciano il sito. Mentre un utilizzo improprio dello stato della sessione può incidere sulle prestazioni siti di grandi dimensioni, l'uso di luce funzionerà anche per scopi dimostrativi.*

La classe ShoppingCart espone i metodi seguenti:

**AddToCart** accetta un Album come parametro e lo aggiunge al carrello dell'utente. Poiché la tabella di carrello tiene traccia della quantità per ogni album, include la logica per creare una nuova riga, se necessario o semplicemente incrementa la quantità se l'utente ha già ordinato una copia dell'album.

**RemoveFromCart** accetta un ID Album e di rimuoverlo dal carrello dell'utente. Se l'utente dispone solo di una copia dell'album nel carrello, la riga viene rimossa.

**EmptyCart** rimuove tutti gli elementi dal carrello acquisti dell'utente.

**GetCartItems** recupera un elenco di CartItems per la visualizzazione o l'elaborazione.

**GetCount** recupera un numero totale di album dispone di un utente nel carrello acquisti.

**GetTotal** calcola il costo totale di tutti gli elementi nel carrello.

**CreateOrder** converte il carrello acquisti in un ordine durante la fase di estrazione.

**GetCart** è un metodo statico che consente di utilizzare il controller ottenere un oggetto del carrello. Usa il **GetCartId** metodo per gestire la lettura di CartId dalla sessione dell'utente. Il metodo GetCartId richiede HttpContextBase in modo che è possibile leggere CartId dell'utente dalla sessione dell'utente.

Ecco l'intero **classe ShoppingCart**:

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a>ViewModel

Il Controller del carrello acquisti sarà necessario comunicare alcune informazioni complesse alle visualizzazioni che non eseguono correttamente il mapping per gli oggetti modello. Non si desidera modificare i modelli per soddisfare il nostro viste. Le classi del modello devono rappresentare il dominio, non l'interfaccia utente. Una soluzione, è possibile passare le informazioni per il nostro viste utilizzando la classe ViewBag, come è stato eseguito con le informazioni di gestione di Store elenco a discesa, ma il passaggio di molte informazioni tramite ViewBag Ottiene difficile da gestire.

Una soluzione consiste nell'usare il *ViewModel* modello. Quando si utilizza questo modello che è creare classi fortemente tipizzate che sono ottimizzati per gli scenari di visualizzazione ed ed espongono le proprietà per i valori o contenuto dinamico necessari per i modelli di visualizzazione. La classi controller quindi compilare e passare queste classi con ottimizzazione per la visualizzazione per il modello di visualizzazione da utilizzare. In questo modo l'indipendenza dai tipi, il controllo in fase di compilazione ed editor IntelliSense all'interno di modelli di visualizzazione.

Si creerà due modelli di visualizzazione da utilizzare nel controller nostro carrello acquisti: il ShoppingCartViewModel conterrà il contenuto del carrello acquisti dell'utente e la ShoppingCartRemoveViewModel verrà utilizzato per visualizzare le informazioni di conferma quando l'utente rimuove un elemento dal carrello.

Creare una nuova cartella ViewModel nella radice del progetto per organizzare le informazioni. Fare clic sul progetto, selezionare Aggiungi / nuova cartella.

![](mvc-music-store-part-8/_static/image1.jpg)

Nome della cartella ViewModel.

![](mvc-music-store-part-8/_static/image1.png)

Successivamente, aggiungere la classe ShoppingCartViewModel nella cartella ViewModel. Ha due proprietà: un elenco di articoli del carrello e un valore decimale per contenere il prezzo totale per tutti gli elementi nel carrello.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

Aggiungere ora la ShoppingCartRemoveViewModel alla cartella ViewModel, con le seguenti quattro proprietà.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a>Il Controller del carrello acquisti

Il controller Shopping Cart ha tre scopi principali: aggiunta di elementi a un carrello, la rimozione di elementi dal carrello e visualizzazione degli elementi nel carrello. Apporterà utilizzare delle tre classi a cui è appena creato: ShoppingCartViewModel ShoppingCartRemoveViewModel e ShoppingCart. Come nel StoreController e StoreManagerController, verrà aggiunto un campo deve contenere un'istanza di MusicStoreEntities.

Aggiungere un nuovo controller Shopping Cart al progetto utilizzando il modello di controller vuoto.

![](mvc-music-store-part-8/_static/image2.png)

Di seguito è riportato il ShoppingCart Controller completo. Le azioni Aggiungi Controller e di indice dovrebbero essere molto familiare. Le azioni del controller Remove e CartSummary gestiscono due casi speciali, che verranno discussi nella sezione seguente.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a>Aggiornamenti AJAX con jQuery

Successivamente, si creerà una pagina di indice di carrello acquisti che deve essere fortemente tipizzata per il ShoppingCartViewModel e utilizza il modello di visualizzazione elenco utilizzando lo stesso metodo.

![](mvc-music-store-part-8/_static/image3.png)

Tuttavia, anziché utilizzare un HTML. ActionLink per rimuovere gli elementi dal carrello, useremo jQuery per "associare" l'evento click per tutti i collegamenti in questa vista con la classe HTML RemoveLink. Anziché il modulo di registrazione, questo gestore dell'evento click consentirà solo un callback AJAX per l'azione del controller RemoveFromCart. Il RemoveFromCart restituisce un risultato serializzato in formato JSON, che il callback di jQuery quindi analizza ed esegue quattro aggiornamenti rapidi della pagina tramite jQuery:

- 1. Rimuove il album eliminato dall'elenco
- 2. Aggiorna il conteggio di carrello nell'intestazione
- 3. Visualizza un messaggio di aggiornamento all'utente
- 4. Aggiorna il prezzo totale carrello

Poiché lo scenario di installazione viene gestito da un callback Ajax all'interno della visualizzazione dell'indice, non è necessario un'ulteriore visualizzazione disponibile per l'azione RemoveFromCart. Ecco il codice completo per la visualizzazione /ShoppingCart/Index:

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

Per testarlo, è necessario essere in grado di aggiungere elementi al nostro carrello acquisti. Verrà aggiornata la **dettagli archivio** visualizzazione per includere un pulsante "Aggiungi al carrello". Mentre è attiva la, possiamo includere alcune delle informazioni aggiuntive che è stato aggiunto dopo è ultimo aggiornamento di questa visualizzazione: Genre, artista, prezzo e dell'album. Il codice di visualizzazione Dettagli archivio aggiornato viene visualizzato come illustrato di seguito.

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

Ora è possibile fare clic tramite lo store e test aggiunta e rimozione di album da e verso il nostro carrello acquisti. Eseguire l'applicazione e individuare l'indice dell'archivio.

![](mvc-music-store-part-8/_static/image4.png)

Fare clic su un genere per visualizzare un elenco di album.

![](mvc-music-store-part-8/_static/image5.png)

Facendo clic sul titolo di un Album ora mostra la visualizzazione dettagli Album aggiornata, incluso il pulsante "Aggiungi al carrello".

![](mvc-music-store-part-8/_static/image6.png)

Fare clic sul pulsante "Aggiungi al carrello" Mostra la visualizzazione di indice carrello acquisti con l'elenco di riepilogo del carrello acquisti.

![](mvc-music-store-part-8/_static/image7.png)

Dopo il caricamento di carrello, è possibile fare clic sul pulsante Rimuovi dal relativo collegamento per visualizzare l'aggiornamento di Ajax al carrello acquisti.

![](mvc-music-store-part-8/_static/image8.png)

È stata realizzata un lavoro carrello che consente agli utenti di annullare la registrazione aggiungere elementi al carrello degli acquisti. Nella sezione seguente, in modo per registrare e completare il processo di estrazione.


> [!div class="step-by-step"]
> [Precedente](mvc-music-store-part-7.md)
> [Successivo](mvc-music-store-part-9.md)
