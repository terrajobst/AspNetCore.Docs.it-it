---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'Parte 9: Registrazione ed estrazione | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Parte 9 vengono illustrate la registrazione e l'estrazione.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: 935729be0dce790c6fce2e2e982ee063318d64dd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367010"
---
<a name="part-9-registration-and-checkout"></a>Parte 9: Registrazione ed estrazione
====================
da [Jon Galloway](https://github.com/jongalloway)

> Store la musica MVC è un'applicazione di esercitazione introduce e dettagliata spiega come usare ASP.NET MVC e Visual Studio per lo sviluppo web.  
>   
> La Store musica MVC è un'implementazione di archivio di esempio leggero che vende gli album musicali online e ne implementa amministrazione sito di base, accesso dell'utente e la funzionalità di carrello acquisti.  
>   
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Parte 9 vengono illustrate la registrazione e l'estrazione.


In questa sezione verranno creati un CheckoutController che raccoglierà le informazioni di pagamento e indirizzo dell'acquirente. Sarà necessario agli utenti di registrarsi con il nostro sito prima dell'estrazione, in modo che questo controller richiederà l'autorizzazione.

Gli utenti si sposterà al processo di estrazione dal carrello acquisti facendo clic sul pulsante "Checkpoint".

![](mvc-music-store-part-9/_static/image1.jpg)

Se l'utente non è connesso, verrà chiesto.

![](mvc-music-store-part-9/_static/image1.png)

Dopo l'accesso, all'utente viene quindi visualizzato il pagamento e indirizzo nella visualizzazione.

![](mvc-music-store-part-9/_static/image2.png)

Una volta che il modulo compilato e ha inviato l'ordine, verrà visualizzata la schermata di conferma dell'ordine.

![](mvc-music-store-part-9/_static/image3.png)

Prova a visualizzare un ordine non esistente o un ordine in cui non appartiene all'utente visualizzerà la visualizzazione di errore.

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a>Eseguire la migrazione del carrello degli acquisti

Durante il processo di acquisto è anonimo, quando l'utente fa clic sul pulsante con checkpoint, sarà necessario registrare ed eseguire l'accesso. Gli utenti si aspettano che si manterrà le informazioni relative al carrello acquisti tra visite, pertanto è necessario associare le informazioni relative al carrello acquisti un utente quando vengono completate registrazione o accesso.

Si tratta in realtà molto semplice, come la classe ShoppingCart già dispone di un metodo quale assocerà tutti gli elementi nel carrello corrente con un nome utente. È necessario semplicemente chiamare questo metodo quando un utente ha completato la registrazione o accesso.

Aprire il **AccountController** classe che abbiamo aggiunto quando si stavamo si configura l'appartenenza e l'autorizzazione. Aggiungere un usando l'istruzione che fa riferimento MvcMusicStore.Models, quindi aggiungere il metodo MigrateShoppingCart seguente:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

Successivamente, modificare l'azione di post di accesso per chiamare MigrateShoppingCart dopo che l'utente è stato convalidato, come illustrato di seguito:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

Apportare la stessa modifica al registro Registra azione, subito dopo è stato creato l'account utente:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

Questo è tutto - è ora un carrello acquisti anonimo verrà automaticamente trasferito a un account utente al termine della registrazione o accesso.

## <a name="creating-the-checkoutcontroller"></a>Creazione di CheckoutController

Fare doppio clic sulla cartella controller e aggiungere un nuovo Controller al progetto denominato CheckoutController usando il modello controller vuoto.

![](mvc-music-store-part-9/_static/image5.png)

In primo luogo, aggiungere l'attributo Authorize sopra la dichiarazione di classe Controller in modo da richiedere agli utenti di registrarsi prima dell'estrazione:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

*Nota: È simile alla modifica che sono state apportate in precedenza al StoreManagerController, ma in tal caso l'attributo Authorize richiesto che l'utente sia in un ruolo di amministratore. Nel Controller di estrazione, si sta richiedendo l'utente di connettersi, ma che non richiedono che si trovino gli amministratori.*

Per ragioni di semplicità, Microsoft non sarà a che fare con le informazioni di pagamento in questa esercitazione. Al contrario, si consente agli utenti di estrarre usando un codice promozionale. Si archivierà il codice promozionale tramite una costante denominata codice di promozione.

Come in StoreController, si dichiara un campo per conservare un'istanza della classe MusicStoreEntities, denominata storeDB. Per rendere utilizza la classe MusicStoreEntities, è necessario aggiungere using istruzione dello spazio dei nomi MvcMusicStore.Models. Di seguito è riportata la parte superiore del controller Checkout.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

Il CheckoutController avrà le azioni del controller seguente:

**AddressAndPayment (GET method)** verrà visualizzato un form per consentire all'utente di immettere le informazioni.

**AddressAndPayment (metodo POST)** verrà convalidato l'input e di elaborare l'ordine.

**Completa** verranno visualizzati dopo che un utente viene completato il processo di estrazione. Questa visualizzazione includerà numero d'ordine dell'utente, come conferma.

In primo luogo, è possibile rinominare l'azione Index del controller (che è stata generata durante la creazione del controller) in AddressAndPayment. Questa azione del controller Visualizza solo il modulo di estrazione, in modo che non richieda le informazioni sul modello.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

Il metodo POST AddressAndPayment seguirà lo stesso modello abbiamo utilizzato la StoreManagerController: verrà effettuato un tentativo accettare l'invio del form e completare l'ordine e verrà nuovamente visualizzato il form in caso di errore.

Dopo la convalida dell'input di modulo soddisfa i requisiti di convalida per un ordine, si controllerà direttamente il valore di modulo di codice di promozione. Presupponendo che tutto sia corretto, che si salverà le informazioni aggiornate con l'ordine, indicare l'oggetto ShoppingCart per completare il processo dell'ordine e il reindirizzamento all'azione di completamento.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

Al completamento del processo di estrazione, gli utenti verranno reindirizzati per l'azione del controller completo. Questa azione eseguirà un controllo semplice per verificare che l'ordine appartenga effettivamente l'utente ha eseguito l'accesso prima di visualizzare il numero dell'ordine come un messaggio di conferma.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

*Nota: La visualizzazione di errore è stata creata automaticamente per noi nella cartella /Views/Shared quando abbiamo iniziato con il progetto.*

Il codice CheckoutController completo è come segue:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a>Aggiunta della visualizzazione AddressAndPayment

A questo punto, è possibile creare la vista AddressAndPayment. Fare clic su una delle azioni controller AddressAndPayment e aggiungere una visualizzazione denominata AddressAndPayment che è fortemente tipizzata come un ordine e Usa il modello di modifica, come illustrato di seguito.

![](mvc-music-store-part-9/_static/image6.png)

In questa vista renderà usare due delle tecniche di cui è stato esaminato durante la compilazione della visualizzazione StoreManagerEdit:

- Si userà Html.EditorForModel() per visualizzare i campi del modulo per il modello di ordine
- Si userà le regole di convalida utilizzando una classe Order con gli attributi di convalida

Si inizierà aggiornando il codice del modulo per l'uso Html.EditorForModel(), seguito da una casella di testo aggiuntiva per il codice promozionale. Seguito è riportato il codice completo per la visualizzazione AddressAndPayment.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a>Definizione delle regole di convalida per l'ordine

Ora che la vista è configurato, si configurerà le regole di convalida per il nostro modello ordine come in precedenza per il modello di Album. Fare doppio clic sulla cartella modelli e aggiungere una classe denominata dell'ordine. Oltre agli attributi di convalida che dell'album usati in precedenza, abbiamo inoltre utilizzerà un'espressione regolare per convalidare l'indirizzo di posta elettronica dell'utente.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

Tentativo di inviare il modulo con mancanti o informazioni non valide a questo punto verranno visualizzato il messaggio di errore utilizzando la convalida lato client.

![](mvc-music-store-part-9/_static/image7.png)

Bene, abbiamo eseguito la maggior parte del lavoro per il processo di estrazione. abbiamo appena pochi odds and termina alla fine. È necessario aggiungere due visualizzazioni semplici ed è necessario occuparsi di consegna delle informazioni carrello durante il processo di accesso.

## <a name="adding-the-checkout-complete-view"></a>Aggiunta della visualizzazione completa di estrazione

La vista completa di estrazione è piuttosto semplice, poiché è sufficiente visualizzare l'ordine di ID. Fare doppio clic sull'azione di completamento controller e aggiungere una visualizzazione denominata completa che è fortemente tipizzata come valore int.

![](mvc-music-store-part-9/_static/image8.png)

A questo punto si aggiornerà il codice di visualizzazione per visualizzare l'ID dell'ordine, come illustrato di seguito.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a>Aggiornamento della vista dell'errore

Il modello predefinito include una visualizzazione di errori nella cartella views condiviso in modo che possa essere usato nuovamente in un' posizione nel sito. In questa vista di errore contiene un errore molto semplice e non usa il nostro sito Layout, in modo che verrà aggiornata in.

Poiché si tratta di una pagina di errore generico, il contenuto è molto semplice. Verrà incluso un messaggio e un collegamento per passare alla pagina precedente nella cronologia se l'utente vuole riprovare l'azione.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


> [!div class="step-by-step"]
> [Precedente](mvc-music-store-part-8.md)
> [Successivo](mvc-music-store-part-10.md)
