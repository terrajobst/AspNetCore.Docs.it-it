---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'Parte 5: Logica di Business | Documenti Microsoft'
author: JoeStagner
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 5 aggiunge una logica di business.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: e205788e05a2ad94d86d4847c11c40898b1c3113
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="part-5-business-logic"></a>Parte 5: Logica di Business
====================
da [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks viene illustrato come particolarmente semplice è creare potenti applicazioni scalabili per la piattaforma .NET. Illustra come usare le nuove caratteristiche in ASP.NET 4 per creare un archivio online, inclusi gli acquisti, estrazione e l'amministrazione.
> 
> Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 5 aggiunge una logica di business.


## <a id="_Toc260221671"></a>Aggiunta di una logica di Business

Si vuole che la nostra esperienza acquisto sia disponibile ogni volta che un utente visita il sito web. Visitatori sarà in grado di cercare e aggiungere elementi al carrello, anche se non sono registrati o registrati. Quando sono pronti per l'estrazione essi verrà assegnato l'opzione di autenticazione e se non sono ancora membri saranno in grado di creare un account.

Ciò significa che è necessario implementare la logica per convertire il carrello acquisti da uno stato anonimo in uno stato "Registered User".

Si crea una directory denominata "Classi" quindi pulsante destro del mouse sulla cartella e creare un nuovo file "Class" denominato MyShoppingCart.cs

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

Come accennato in precedenza si sarà l'estensione della classe che implementa la pagina MyShoppingCart.aspx lo faremo questo utilizzo. Costrutto potente "classe parziale" di NET.

La chiamata per il file MyShoppingCart.aspx.cf generata è simile al seguente.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

Si noti l'uso della parola chiave "parziale".

Il file di classe generata è simile al seguente:

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

Si consentirà di unire il nostro implementazioni aggiungendo la parola chiave partial anche questo file.

Il nuovo file di classe è ora simile al seguente.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

Il primo metodo che saranno aggiunti alla classe il metodo "AddItem". Si tratta del metodo che, in definitiva, verrà chiamato quando l'utente fa clic sui collegamenti di "Aggiunta di immagini" nelle pagine di elenco di prodotti e i dettagli sul prodotto.

Aggiungere la seguente utilizzando le istruzioni nella parte superiore della pagina.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

E aggiungere questo metodo alla classe MyShoppingCart.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

Si sta usando LINQ to Entities per vedere se l'elemento è già nel carrello. Se in tal caso, si aggiorna la quantità dell'ordine dell'elemento, in caso contrario è creare una nuova voce per l'elemento selezionato

Per chiamare questo metodo verrà implementata una pagina AddToCart.aspx che non solo il metodo della classe ma quindi visualizzati carrello acquisti un = dopo aver aggiunto l'elemento corrente.

Fare clic sul nome della soluzione in Esplora soluzioni e aggiungere e nuova pagina denominata AddToCart.aspx come è stato fatto in precedenza.

Mentre è possibile utilizzare questa pagina per visualizzare i risultati intermedi come bassi problemi scorte e così via, nell'implementazione, la pagina verrà effettivamente non esegue il rendering, ma piuttosto chiamare la logica di "Aggiungi" e reindirizzare.

A questo scopo verrà aggiunto il codice seguente alla pagina\_evento di caricamento.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

Si noti che stiamo cercando il prodotto da aggiungere al carrello acquisti di un parametro di stringa di query e la chiamata al metodo AddItem la classe.

Supponendo che non gli errori vengono rilevati il controllo viene passato alla pagina SHoppingCart.aspx verrà completamente implementato successivamente. Se deve essere un errore, viene generata un'eccezione.

Attualmente non è stata ancora implementata un gestore errori globale diventano non gestita per l'applicazione di questa eccezione, ma ci consentirà di risolvere questo a breve.

Si noti inoltre l'utilizzo dell'istruzione Debug.Fail() (disponibili tramite`using System.Diagnostics;)`

È l'applicazione è in esecuzione nel debugger, questo metodo verrà visualizzata una finestra di dialogo dettagliata con informazioni sullo stato delle applicazioni insieme al messaggio di errore che si specifica.

Quando in esecuzione nell'ambiente di produzione l'istruzione Debug.Fail() viene ignorato.

Si noterà nel codice sopra una chiamata a un metodo in ai nomi di classe di carrello acquisti "GetShoppingCartId".

Aggiungere il codice per implementare il metodo come indicato di seguito.

Si noti che è stato aggiunto anche pulsanti di aggiornamento e l'estrazione e un'etichetta in cui è possibile visualizzare il carrello "totale".

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

È ora possibile aggiungere elementi al nostro carrello acquisti ma non è stata implementata la logica per visualizzare il carrello dopo l'aggiunta di un prodotto.

In tal caso, nella pagina MyShoppingCart.aspx verrà aggiunto un controllo EntityDataSource e un controllo GridVire come indicato di seguito.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

Chiamare il form nella finestra di progettazione in modo che è possibile fare doppio clic sul pulsante di aggiornamento del carrello e generare il gestore eventi click che viene specificato nella dichiarazione nel markup.

I dettagli verrà implementato in un secondo momento, ma in questo modo verrà compilata ed eseguita l'applicazione senza errori di segnalare il problema.

Quando si esegue l'applicazione e aggiungere un elemento al carrello acquisti verrà visualizzato.

![](tailspin-spyworks-part-5/_static/image2.jpg)

Si noti che è stato deviato dalla visualizzazione griglia "default" mediante l'implementazione di tre colonne personalizzate.

Il primo è un oggetto modificabile, il campo "Associato" per la quantità:

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

Alla successiva è una colonna "calcolata" che visualizza il totale della voce (l'elemento di costo per la quantità da ordinare):

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

Infine, è disponibile una colonna personalizzata che contiene un controllo casella di controllo che l'utente utilizzerà per indicare che l'elemento deve essere rimosso dal grafico acquisto.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

Come si può notare, l'ordine di riga totale è vuoto a questo punto aggiungere logica per calcolare il totale di ordini.

È prima viene implementato un metodo "GetTotal" alla classe MyShoppingCart.

Nel file MyShoppingCart.cs aggiungere il codice seguente.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

Quindi, nella pagina\_gestore di eventi di caricamento si sarà possibile chiamare il metodo GetTotal. Un test per vedere se il carrello acquisti è vuoto e regolare la visualizzazione di conseguenza se verrà aggiunto allo stesso tempo.

A questo punto il carrello acquisti è vuoto si ottiene questo:

![](tailspin-spyworks-part-5/_static/image4.jpg)

In caso contrario, è e il totale.

![](tailspin-spyworks-part-5/_static/image5.jpg)

Tuttavia, questa pagina non è ancora completa.

È necessario logica aggiuntiva per ricalcolare il carrello acquisti, rimuovendo gli elementi contrassegnati per la rimozione e determinando i nuovi valori quantità come alcune sia stato modificato nella griglia dall'utente.

Consente di aggiungere un metodo "RemoveItem" per la classe di carrello in MyShoppingCart.cs per gestire il caso, quando si contrassegna un elemento per la rimozione.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

Ora si ad un metodo per gestire il caso, quando un utente cambia semplicemente la qualità verrà ordinata in GridView.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

Con le funzionalità base di installazione e aggiornamento sul posto è possibile implementare la logica che effettivamente Aggiorna carrello nel database. (In MyShoppingCart.cs)

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

È possibile notare che questo metodo prevede due parametri. Uno è il carrello acquisti Id e l'altro è una matrice di oggetti di tipo definito dall'utente.

In modo da ridurre al minimo la dipendenza del nostro logica sulle specifiche di interfaccia utente, viene definito una struttura di dati che è possibile utilizzare per passare gli elementi del carrello acquisti al codice senza il necessità di accedere direttamente ai controllo GridView (metodo).

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

Nel file MyShoppingCart.aspx.cs è possibile usare questa struttura in questo gestore dell'evento fare clic su pulsante di aggiornamento come indicato di seguito. Si noti che oltre ad aggiornare il carrello verrà aggiornato anche il totale di carrello.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

Si noti con particolare interesse questa riga di codice:

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

GetValues() è una funzione di supporto speciale che verrà implementato in MyShoppingCart.aspx.cs come indicato di seguito.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

Ciò fornisce un modo pulito per accedere ai valori degli elementi associati in del controllo GridView. Poiché non è associato il controllo casella di controllo "Rimuovi elemento" sarà accedere tramite il metodo FindControl().

In questa fase di sviluppo del progetto è stiamo preparativi per l'implementazione del processo di estrazione.

Prima di procedere si utilizzare Visual Studio per generare il database delle appartenenze e aggiungere un utente nel repository di appartenenza.

>[!div class="step-by-step"]
[Precedente](tailspin-spyworks-part-4.md)
[Successivo](tailspin-spyworks-part-6.md)
