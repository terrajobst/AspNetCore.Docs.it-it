---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Parte 8: Carrello con aggiornamenti Ajax | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Parte 8 copre carrello con aggiornamenti Ajax.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: cab338e56505c453532a26d794eb7bf4e94555a9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826908"
---
<a name="part-8-shopping-cart-with-ajax-updates"></a>Parte 8: Carrello con aggiornamenti Ajax
====================
da [Jon Galloway](https://github.com/jongalloway)

> Store la musica MVC è un'applicazione di esercitazione introduce e dettagliata spiega come usare ASP.NET MVC e Visual Studio per lo sviluppo web.  
>   
> La Store musica MVC è un'implementazione di archivio di esempio leggero che vende gli album musicali online e ne implementa amministrazione sito di base, accesso dell'utente e la funzionalità di carrello acquisti.  
>   
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Parte 8 copre carrello con aggiornamenti Ajax.


Sarà consentito agli utenti di inserire gli album nel carrello acquisti senza eseguire la registrazione, ma sarà necessario registrarsi come utenti guest a completare il checkpoint. Il processo di estrazione e acquisti sarà separato in due controller: un ShoppingCart Controller che consente di aggiungere in modo anonimo elementi al carrello e un Controller di estrazione che gestisce il processo di estrazione. Si sarà il carrello della spesa in questa sezione per iniziare, quindi compilare il processo di estrazione nella sezione seguente.

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a>Aggiunta di classi del modello carrello della spesa, l'ordine e OrderDetail

I processi carrello della spesa e l'estrazione renderà usare alcune nuove classi. Fare doppio clic su cartella Models e aggiungere una classe di carrello (Cart.cs) con il codice seguente.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

Questa classe è molto simile ad altri utenti che abbiamo usato finora, fatta eccezione per l'attributo [Key] per la proprietà RecordId. Gli elementi del carrello avrà un identificatore di stringa denominato CartID per consentire di acquisti anonimi, ma la tabella include una chiave primaria integer denominata RecordId. Per convenzione, Entity Framework Code First si aspetta che la chiave primaria per una tabella denominata carrello sarà CartId o ID, ma possiamo facilmente eseguito l'override che tramite le annotazioni o codice se si desidera. Questo è un esempio di come è possibile usare le convenzioni semplice in Entity Framework Code First quando si rivelano adatti alle di Stati Uniti, ma è non stiamo vincolati dalla loro quando non è così.

Successivamente, aggiungere una classe Order (Order.cs) con il codice seguente.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

Questa classe tiene traccia delle informazioni di riepilogo e il recapito di un ordine. **Non verrà compilata ancora**perché contiene una proprietà di navigazione OrderDetails che dipende da una classe non ne abbiamo creati. Correggiamo che ora mediante l'aggiunta di una classe denominata OrderDetail.cs, aggiungendo il codice seguente.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

Ci assicureremo ultimo aggiornamento alla nostra classe MusicStoreEntities includere DbSet che espongono queste nuove classi di modello, incluso anche un elemento DbSet&lt;artista&gt;. La classe MusicStoreEntities aggiornata viene visualizzata come di seguito.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a>Gestire la logica di business carrello della spesa

Successivamente, si creerà la classe ShoppingCart nella cartella Models. Il modello ShoppingCart gestisce l'accesso ai dati nella tabella carrello della spesa. Inoltre, gestirà la logica di business a per l'aggiunta e rimozione di elementi del carrello.

Poiché non si desidera richiedere agli utenti di iscriversi a un account aggiungere elementi al carrello acquisti, verranno assegnate agli utenti un identificatore univoco temporaneo (tramite un GUID o identificatore univoco globale) quando accedono a carrello acquisti. Verrà archiviato questo ID usando la classe di sessione di ASP.NET.

*Nota: La sessione ASP.NET è un modo pratico per archiviare le informazioni specifiche dell'utente che scadranno dopo che gli utenti lasciano il sito. Mentre un utilizzo improprio dello stato della sessione può avere implicazioni sulle prestazioni in siti di grandi dimensioni, utilizzo chiaro funzionerà bene per scopi dimostrativi.*

La classe ShoppingCart espone i metodi seguenti:

**AddToCart** accetta un Album come parametro e lo aggiunge al carrello dell'utente. Poiché la tabella carrello tiene traccia quantity per ogni album, include la logica per creare una nuova riga, se necessario o semplicemente incrementare la quantità se l'utente è già ordinata una copia dell'album.

**RemoveFromCart** accetta un ID di Album e lo rimuove dal carrello dell'utente. Se l'utente avesse solo una copia dell'album nel carrello acquisti, la riga viene rimossa.

**EmptyCart** rimuove tutti gli articoli dal carrello acquisti dell'utente.

**GetCartItems** recupera un elenco di CartItems per visualizzazione o l'elaborazione.

**GetCount** recupera un numero totale di album di un utente ha inseriti nel carrello.

**GetTotal** calcola il costo totale di tutti gli elementi nel carrello.

**CreateOrder** converte il carrello acquisti in un ordine durante la fase di completamento della transazione.

**GetCart** è un metodo statico che consente ai controller ottenere un oggetto del carrello. Usa il **GetCartId** metodo per gestire la lettura di CartId dalla sessione dell'utente. Il metodo GetCartId richiede HttpContextBase in modo che sia possibile leggerlo CartId dell'utente dalla sessione dell'utente.

Di seguito è riportato l'intero **ShoppingCart classe**:

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a>ViewModel

Il Controller del carrello della spesa necessario comunicare alcune informazioni per le relative visualizzazioni complesse che non viene mappato correttamente per gli oggetti modello. Non si desidera modificare i modelli in base alle viste; Le classi del modello devono rappresentare il dominio, non l'interfaccia utente. Una soluzione sarebbe per passare le informazioni relative alle viste utilizzando la classe ViewBag, come abbiamo fatto con le informazioni sull'elenco a discesa Store Manager, ma passando una grande quantità di informazioni tramite ViewBag diventa difficile da gestire.

Risolvere questo problema consiste nell'usare la *ViewModel* pattern. Quando si usa questo modello è creare classi fortemente tipizzate che sono ottimizzati per gli scenari di visualizzazione specifici ed ed espongono le proprietà per i valori/contenuto dinamico necessari per i modelli di visualizzazione. Le classi controller possono quindi popolare e passare queste classi ottimizzate per la visualizzazione al nostro modello di visualizzazione da utilizzare. In questo modo l'indipendenza dai tipi, controllo della fase di compilazione ed editor IntelliSense all'interno dei modelli di visualizzazione.

Creeremo due modelli di visualizzazione da utilizzare nel controller del carrello degli acquisti: il ShoppingCartViewModel conterrà il contenuto del carrello acquisti dell'utente e la ShoppingCartRemoveViewModel verrà usato per visualizzare le informazioni di conferma quando l'utente rimuove un elemento dal carrello acquisti.

Creare una nuova cartella ViewModel nella radice del progetto per organizzare le informazioni. Fare clic sul progetto, scegliere Aggiungi / nuova cartella.

![](mvc-music-store-part-8/_static/image1.jpg)

Denominare la cartella ViewModel.

![](mvc-music-store-part-8/_static/image1.png)

Successivamente, aggiungere la classe ShoppingCartViewModel nella cartella ViewModel. Ha due proprietà: un elenco di elementi del carrello e un valore decimale per contenere il prezzo totale per tutti gli elementi nel carrello.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

Ora aggiungere il ShoppingCartRemoveViewModel nella cartella ViewModel, con le seguenti quattro proprietà.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a>Il Controller del carrello acquisti

Il controller del carrello della spesa ha tre scopi principali: aggiunta di elementi al carrello, rimozione di elementi dal carrello della spesa e la visualizzazione di elementi nel carrello. Useranno le tre classi abbiamo appena creato: ShoppingCartViewModel ShoppingCartRemoveViewModel e ShoppingCart. Come nel StoreController e StoreManagerController, si aggiungerà un campo deve contenere un'istanza di MusicStoreEntities.

Aggiungere un nuovo controller di carrello della spesa per il progetto usando il modello controller vuoto.

![](mvc-music-store-part-8/_static/image2.png)

Ecco il ShoppingCart Controller completo. Le azioni dell'indice e Aggiungi Controller dovrebbero risultare familiare. Le azioni del controller Remove e CartSummary gestiscono due casi speciali, che saranno illustrate nella sezione seguente.

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a>Aggiornamenti AJAX con jQuery

Successivamente si creerà una pagina di indice carrello della spesa che è fortemente tipizzata per il ShoppingCartViewModel e Usa il modello di visualizzazione elenco usando il metodo di stesso come prima.

![](mvc-music-store-part-8/_static/image3.png)

Tuttavia, anziché utilizzare un HTML. ActionLink per rimuovere gli articoli dal carrello, useremo jQuery per "connettere" l'evento click per tutti i collegamenti in questa vista con la classe HTML RemoveLink. Anziché il modulo di registrazione, questo gestore dell'evento click renderà semplicemente un callback AJAX per l'azione del controller RemoveFromCart. Il RemoveFromCart restituisce un risultato JSON serializzato, quali i callback di jQuery quindi analizza ed esegue quattro aggiornamenti rapidi per la pagina tramite jQuery:

- 1. Rimuove il album eliminato dall'elenco
- 2. Aggiorna il conteggio di carrello nell'intestazione
- 3. Visualizza un messaggio di aggiornamento all'utente
- 4. Aggiorna il prezzo totale del carrello

Poiché lo scenario di installazione viene gestito da un callback Ajax nella visualizzazione di indice, non occorre un'ulteriore visualizzazione per l'azione RemoveFromCart. Ecco il codice completo per la visualizzazione /ShoppingCart/Index:

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

Per testarlo, è necessario essere in grado di aggiungere elementi al nostro carrello acquisti. Aggiorneremo nostri **Store dettagli** vista da includere un pulsante "Aggiungi al carrello". Mentre ci troviamo, possiamo includere alcune informazioni aggiuntive Album che è stata aggiunta perché abbiamo aggiornato in questa vista: Genre, artista, prezzo e dell'album. Il codice di visualizzazione dei dettagli Store aggiornato viene visualizzato come illustrato di seguito.

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

Ora è possibile fare clic tramite lo store e testare l'aggiunta e rimozione di album da e verso il carrello acquisti. Eseguire l'applicazione e passare alla corrispondenza dell'indice Store.

![](mvc-music-store-part-8/_static/image4.png)

Successivamente, fare clic su un genere per visualizzare un elenco di album.

![](mvc-music-store-part-8/_static/image5.png)

Facendo clic su un titolo Album ora mostra la visualizzazione dei dettagli di Album aggiornato, incluso il pulsante "Aggiungi al carrello".

![](mvc-music-store-part-8/_static/image6.png)

Facendo clic sul pulsante "Aggiungi al carrello" Mostra la visualizzazione di indice carrello degli acquisti con l'elenco di riepilogo del carrello acquisti.

![](mvc-music-store-part-8/_static/image7.png)

Dopo aver caricato il carrello, è possibile fare clic sul pulsante Rimuovi dal relativo collegamento per visualizzare l'aggiornamento di Ajax al carrello acquisti.

![](mvc-music-store-part-8/_static/image8.png)

Abbiamo creato un lavoro carrello della spesa che consente agli utenti non registrati aggiungere elementi al carrello acquisti. Nella sezione seguente, sarà consentito loro di registrare e completare il processo di estrazione.


> [!div class="step-by-step"]
> [Precedente](mvc-music-store-part-7.md)
> [Successivo](mvc-music-store-part-9.md)
