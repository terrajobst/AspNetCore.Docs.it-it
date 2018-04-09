---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'Parte 9: Estrazione e registrazione | Documenti Microsoft'
author: jongalloway
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio. Parte 9 riguarda la registrazione e estrazione.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: e7e83b70f2508b6dfc0c078b992747a76e4d0ff2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="part-9-registration-and-checkout"></a>Parte 9: Estrazione e registrazione
====================
da [Jon Galloway](https://github.com/jongalloway)

> L'archivio di musica MVC è un'applicazione di esercitazione che vengono presentati e dettagliate sull'utilizzo di MVC ASP.NET e Visual Studio per lo sviluppo web.  
>   
> L'archivio di musica MVC è un'implementazione dell'archivio di esempio semplice che vende album musicali online e ne implementa amministrazione sito di base, account utente e funzionalità di carrello acquisti.  
>   
> Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio. Parte 9 riguarda la registrazione e estrazione.


In questa sezione, che verrà creata una CheckoutController che raccoglierà informazioni di pagamento e indirizzo dell'acquirente. Si richiederà agli utenti di registrare il sito prima dell'estrazione, in modo che il controller richiederà l'autorizzazione.

Gli utenti si sposterà il processo di estrazione dal carrello acquisti facendo clic sul pulsante "Checkpoint".

![](mvc-music-store-part-9/_static/image1.jpg)

Se l'utente non è connesso, verrà richiesto di.

![](mvc-music-store-part-9/_static/image1.png)

Dopo l'accesso ha esito positivo, l'utente viene visualizzata la vista indirizzo e pagamento.

![](mvc-music-store-part-9/_static/image2.png)

Dopo che il modulo compilato e inviato l'ordine, essi verranno visualizzati la schermata di conferma dell'ordine.

![](mvc-music-store-part-9/_static/image3.png)

Tenta di visualizzare un ordine non esistente o un ordine che non appartiene all'utente verrà visualizzato l'errore, vedere.

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a>La migrazione il carrello acquisti

Durante il processo di acquisto è anonimo, quando l'utente fa clic sul pulsante di estrazione, sarà necessario per registrare e account di accesso. Gli utenti si aspetteranno che si manterrà le informazioni relative al carrello acquisti tra visite, pertanto è necessario associare le informazioni relative al carrello acquisti un utente al momento della compilazione di registrazione o l'account di accesso.

Questo è effettivamente molto semplice, come la classe ShoppingCart dispone già di un metodo quale assocerà tutti gli elementi nel carrello corrente con un nome utente. È semplicemente necessario chiamare questo metodo quando un utente ha completato la registrazione o l'account di accesso.

Aprire il **AccountController** classe che viene aggiunto quando è durante la configurazione di appartenenza e l'autorizzazione. Aggiungere un utilizzando l'istruzione che fa riferimento MvcMusicStore.Models, quindi aggiungere il metodo MigrateShoppingCart seguente:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

Successivamente, modificare l'azione di post di accesso per chiamare MigrateShoppingCart dopo che l'utente è stato convalidato, come illustrato di seguito:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

Apportare la stessa modifica al Registro di post-azione, immediatamente dopo che l'account utente è stato creato:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

È tutto - ora un carrello acquisti anonimo verrà trasferito automaticamente a un account utente al momento dopo la corretta registrazione o l'account di accesso.

## <a name="creating-the-checkoutcontroller"></a>Creazione di CheckoutController

Pulsante destro del mouse sulla cartella controller e aggiungere un nuovo Controller al progetto denominato CheckoutController utilizzando il modello di controller vuoto.

![](mvc-music-store-part-9/_static/image5.png)

In primo luogo, aggiungere l'attributo Authorize sopra la dichiarazione di classe Controller richiedere agli utenti di registrare prima dell'estrazione:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

*Nota: Questa operazione è simile alla modifica che è apportate in precedenza il StoreManagerController, ma in tal caso l'attributo Authorize necessario che l'utente appartenga a un ruolo di amministratore. Nel Controller di estrazione, è in corso richiedendo all'utente di connettersi ma non richiedono che sono amministratori.*

Per ragioni di semplicità, è non essere gestiscono le informazioni di pagamento in questa esercitazione. Al contrario, si consente agli utenti di estrarre utilizzando un codice promozionale. Verrà archiviato il codice promozionale utilizza una costante denominata codice promozione.

Come StoreController, si dichiara un campo per contenere un'istanza della classe MusicStoreEntities, denominata storeDB. Per rendere usano la classe MusicStoreEntities, sarà necessario aggiungere un tramite l'istruzione per lo spazio dei nomi MvcMusicStore.Models. Di seguito è riportata nella parte superiore di questo controller di estrazione.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

Il CheckoutController saranno disponibili le seguenti azioni controller:

**AddressAndPayment (GET method)** verrà visualizzato un form per consentire all'utente di immettere le informazioni.

**AddressAndPayment (metodo POST)** verrà convalidare l'input ed elaborare l'ordine.

**Completa** verrà visualizzato dopo che un utente ha completato correttamente il processo di estrazione. Questa visualizzazione includerà il numero di ordine dell'utente, conferma.

In primo luogo, rinominare l'azione del controller indice (che è stato generato quando abbiamo creato il controller) per AddressAndPayment. Questa azione del controller Visualizza solo il modulo di estrazione, e pertanto non richiede le informazioni sul modello.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

Il metodo POST AddressAndPayment seguiranno lo stesso modello, abbiamo utilizzato il StoreManagerController: tenterà di accettare l'invio del modulo e completare l'ordine e verrà nuovamente visualizzato il form in caso di errore.

Dopo la convalida dell'input del form soddisfa i requisiti di convalida per un ordine, si verificherà direttamente il valore di modulo di codice promozione. Presupponendo che le informazioni sono corrette, che si salverà le informazioni aggiornate con l'ordine, indicare l'oggetto ShoppingCart per completare il processo dell'ordine e il reindirizzamento all'azione di completamento.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

Al completamento del processo di estrazione, gli utenti verranno reindirizzati all'azione del controller completo. Questa azione esegue un semplice controllo per verificare che l'ordine effettivamente appartengono all'utente connesso prima di visualizzare il numero dell'ordine come una conferma.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

*Nota: La visualizzazione di errore creata automaticamente per noi nella cartella /Views/Shared quando abbiamo iniziato il progetto.*

Il codice CheckoutController completo è il seguente:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a>Aggiunta della visualizzazione AddressAndPayment

A questo punto, creare la vista AddressAndPayment. Fare clic su una delle azioni AddressAndPayment controller e aggiungere una visualizzazione denominata AddressAndPayment è fortemente tipizzata come un ordine che utilizza il modello di modifica, come illustrato di seguito.

![](mvc-music-store-part-9/_static/image6.png)

Questa vista renderà utilizzare due delle tecniche di cui è stato esaminato durante la creazione della visualizzazione StoreManagerEdit:

- Si utilizzerà Html.EditorForModel() per visualizzare i campi del form per il modello di ordine
- Verranno utilizzate le regole di convalida utilizzando una classe Order con gli attributi di convalida

Si inizierà aggiornando il codice del modulo per l'utilizzo di Html.EditorForModel(), seguito da una casella di testo aggiuntiva per il codice promozionale. Il codice completo per la visualizzazione AddressAndPayment è illustrato di seguito.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a>Definizione delle regole di convalida per l'ordine

Dopo che la vista è impostato, si imposterà le regole di convalida per il modello di ordine come è stato fatto in precedenza per il modello di Album. Fare doppio clic sulla cartella modelli e aggiungere una classe denominata ordine. Oltre agli attributi di convalida che è stata utilizzata in precedenza per Album, anche utilizzeremo un'espressione regolare per convalidare l'indirizzo di posta elettronica dell'utente.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

Il tentativo di inviare il form con mancante o informazioni non valide verranno visualizzati il messaggio di errore utilizzando la convalida lato client.

![](mvc-music-store-part-9/_static/image7.png)

Passare alla procedura guidata che abbiamo realizzato la maggior parte del lavoro disco rigido per il processo di estrazione. sono disponibili solo pochi termina in e probabilità per completare. È necessario aggiungere due visualizzazioni semplici, e sono necessarie svolgere il passaggio di informazioni relative al carrello durante il processo di accesso.

## <a name="adding-the-checkout-complete-view"></a>Aggiunta della visualizzazione completa di estrazione

La visualizzazione completa di estrazione è piuttosto semplice, ma è sufficiente visualizzare l'ID di ordine. Fare doppio clic sull'azione di completamento controller e aggiungere una visualizzazione denominata Complete fortemente tipizzato come valore int.

![](mvc-music-store-part-9/_static/image8.png)

Ora verrà aggiornato il codice di visualizzazione per visualizzare l'ID dell'ordine, come illustrato di seguito.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a>Aggiornare la visualizzazione dell'errore

Il modello predefinito include una visualizzazione di errore nella cartella views condiviso in modo che possa essere riutilizzato altrove nel sito. Questa vista di errore contiene un errore molto semplice e non usa il Layout, il sito in modo che verrà aggiornata.

Poiché si tratta di una pagina di errore generico, il contenuto è molto semplice. Verrà incluso un messaggio e un collegamento per passare alla pagina precedente nella cronologia se l'utente desidera riprovare l'azione.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


> [!div class="step-by-step"]
> [Precedente](mvc-music-store-part-8.md)
> [Successivo](mvc-music-store-part-10.md)
