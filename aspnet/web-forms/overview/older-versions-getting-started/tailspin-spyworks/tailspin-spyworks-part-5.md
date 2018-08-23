---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'Parte 5: Logica di Business | Microsoft Docs'
author: JoeStagner
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 5 consente di aggiungere una logica di business.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: e18acb66dbdb3bd3e0dfa21193f617dad82afc74
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836227"
---
<a name="part-5-business-logic"></a>Parte 5: Logica di Business
====================
da [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks viene illustrato come saliente è davvero semplice per creare applicazioni potenti e scalabili per la piattaforma .NET. Illustra come usare le nuove funzionalità in ASP.NET 4 per creare un negozio online, tra cui acquisti, estrazione e l'amministrazione.
> 
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 5 consente di aggiungere una logica di business.


## <a id="_Toc260221671"></a>  Aggiunta di una logica di Business

Si vuole che l'esperienza di shopping siano disponibili ogni volta che un utente visita il sito web Microsoft. I visitatori sarà in grado di individuare e aggiungere elementi al carrello acquisti, anche se non sono registrati o registrati. Quando sono pronte per l'estrazione viene assegnato l'opzione di autenticazione e se non sono ancora membri saranno in grado di creare un account.

Ciò significa che è necessario implementare la logica per convertire il carrello acquisti da uno stato anonimo in uno stato "Registrato User".

È possibile creare una directory denominata "Classi", quindi fare doppio clic sulla cartella e creare un nuovo file "Class" denominato MyShoppingCart.cs

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

Come accennato in precedenza verranno estese della classe che implementa la pagina MyShoppingCart.aspx lo faremo questo utilizzo. Costrutto potente "classe parziale" di NET.

La chiamata per il file MyShoppingCart.aspx.cf generata è simile al seguente:

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

Si noti l'uso della parola chiave "parziale".

Il file di classe generata è simile al seguente:

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

Aggiungendo la parola chiave parziale a anche questo file verranno uniti gli implementazioni.

Il nuovo file di classe è ora simile al seguente:

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

Il primo metodo che verrà aggiunto alla nostra classe è il metodo "AddItem". Questo è il metodo che, in definitiva, verrà chiamato quando l'utente fa clic sui collegamenti "Aggiungi a arte" nelle pagine di elenco dei prodotti e i dettagli sul prodotto.

Aggiungere il codice seguente al usando le istruzioni nella parte superiore della pagina.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

E aggiungere questo metodo alla classe MyShoppingCart.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

Viene usato LINQ to Entities per verificare se l'elemento è già nel carrello. Se pertanto si aggiorna la quantità dell'ordine dell'elemento, in caso contrario, creiamo una nuova voce per l'elemento selezionato

Per chiamare questo metodo verrà implementato una pagina AddToCart.aspx che non solo questo metodo della classe ma quindi visualizzato un = carrello della spesa dopo che è stato aggiunto l'elemento corrente.

Fare clic sul nome della soluzione in Esplora soluzioni e aggiungere e nuova pagina denominata AddToCart.aspx perché abbiamo lavorato in precedenza.

Anche se è stato possibile utilizzare questa pagina per visualizzare i risultati intermedi come i problemi di scorte insufficienti e così via, nell'implementazione, la pagina verrà effettivamente non esegue il rendering, ma piuttosto chiamare la logica di "Aggiungi" e reindirizzare.

Per eseguire questa operazione si aggiungerà il codice seguente alla pagina\_evento di caricamento.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

Si noti che stiamo cercando il prodotto da aggiungere al carrello acquisti da un parametro di stringa di query e la chiamata al metodo AddItem della nostra classe.

Supponendo che non sono errori vengono rilevati il controllo viene passato alla pagina SHoppingCart.aspx che verrà completamente implementato successivamente. Se non vi sarà un errore viene generata un'eccezione.

Attualmente non è stata ancora implementata un gestore errori globale in modo che questa eccezione andrebbe non gestita dall'applicazione ma si sarà risolvere il problema al più presto.

Si noti inoltre l'utilizzo dell'istruzione Debug.Fail() (disponibile tramite `using System.Diagnostics;)`

È l'applicazione viene eseguita all'interno del debugger, questo metodo verrà visualizzata una finestra di dialogo dettagliata con le informazioni sullo stato delle applicazioni insieme al messaggio di errore che si specifica.

Quando in esecuzione nell'ambiente di produzione di Debug.Fail() istruzione verrà ignorata.

Si noterà nel codice sopra una chiamata a un metodo in ai nomi di classe di carrello degli acquisti "GetShoppingCartId".

Aggiungere il codice per implementare il metodo come indicato di seguito.

Si noti che sono stati anche aggiunti i pulsanti di aggiornamento e l'estrazione e un'etichetta in cui è possibile visualizzare il carrello "totale".

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

A questo punto possiamo aggiungere elementi al nostro carrello acquisti, ma non è stata implementata la logica per visualizzare il carrello dopo l'aggiunta di un prodotto.

Quindi, nella pagina MyShoppingCart.aspx si aggiungerà un controllo EntityDataSource e un controllo GridVire come indicato di seguito.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

Chiamare il form nella finestra di progettazione in modo che è possibile fare doppio clic sul pulsante di aggiornamento del carrello e generare il gestore eventi click che viene specificato nella dichiarazione nel markup.

Si implementeranno i dettagli in un secondo momento, ma in questo modo verrà compilata ed eseguita l'applicazione senza errori invia i tuoi commenti.

Quando si esegue l'applicazione e aggiungere un elemento al carrello acquisti verrà visualizzato.

![](tailspin-spyworks-part-5/_static/image2.jpg)

Si noti che è stato deviato dalla visualizzazione griglia "predefinita" mediante l'implementazione di tre colonne personalizzate.

Il primo è un modificabile, il campo "Associato" per la quantità di:

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

Alla successiva è una colonna "calcolata" che visualizza il totale della voce (l'elemento di costo volte la quantità da ordinare):

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

Infine è disponibile una colonna personalizzata che contiene una casella di controllo che l'utente userà per indicare che l'elemento deve essere rimosso dal grafico acquisti.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

Come può notare, l'ordine totale riga è vuoto, pertanto, è possibile aggiungere la logica per la quale calcolare il totale di ordini.

Un metodo "GetTotal" implementeremo prima di tutto alla nostra classe MyShoppingCart.

Nel file MyShoppingCart.cs aggiungere il codice seguente.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

Nella pagina\_può chiamiamo il metodo GetTotal gestore dell'evento Load. Allo stesso tempo si aggiungerà un test per verificare se il carrello acquisti è vuoto e regolare la visualizzazione di conseguenza, se si tratta.

Se il carrello acquisti è vuoto viene visualizzato questo:

![](tailspin-spyworks-part-5/_static/image4.jpg)

E in caso contrario, il totale.

![](tailspin-spyworks-part-5/_static/image5.jpg)

Tuttavia, questa pagina non è ancora completa.

È necessario per la logica aggiuntiva per ricalcolare il carrello acquisti, rimuovendo gli elementi contrassegnati per rimozione e determinare prima di tutto i nuovi valori quantità come alcune sia stato modificato nella griglia dall'utente.

Consente di aggiungere un metodo "RemoveItem" per la classe di carrello in MyShoppingCart.cs per gestire il caso quando un utente di contrassegna gli elementi per la rimozione.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

A questo punto è possibile ad un metodo per gestire il caso quando un utente cambia semplicemente la qualità devono essere ordinati in GridView.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

Con le funzionalità di rimozione e aggiornamento base posto è possibile implementare la logica che aggiorna effettivamente il carrello acquisti nel database. (In MyShoppingCart.cs)

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

Si noterà che questo metodo prevede due parametri. Uno è l'Id carrello acquisti e l'altro è una matrice di oggetti di tipo definito dall'utente.

In modo da ridurre la dipendenza del nostro logica sulle specifiche di interfaccia utente, abbiamo definito una struttura di dati che è possibile usare per passare elementi del carrello acquisti al codice senza il necessità di accedere direttamente al controllo GridView (metodo).

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

Nel nostro file MyShoppingCart.aspx.cs è possibile usare questa struttura nel nostro gestore di eventi fare clic sul pulsante di aggiornamento come indicato di seguito. Si noti che oltre ad aggiornare il carrello verrà aggiornato anche il totale del carrello.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

Si noti di particolare interesse questa riga di codice:

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

GetValues() è una funzione di supporto speciale che verrà implementato in MyShoppingCart.aspx.cs come indicato di seguito.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

Ciò fornisce un metodo chiaro per accedere ai valori degli elementi associati nel nostro controllo GridView. Poiché non è associato il controllo casella di controllo "Rimuovi elemento" si verrà accedervi tramite il metodo FindControl().

In questa fase nello sviluppo del progetto è corso la preparazione all'implementazione del processo di estrazione.

Prima che in questo modo è possibile usare Visual Studio per generare il database di appartenenza e aggiungere un utente nel repository di appartenenza.

> [!div class="step-by-step"]
> [Precedente](tailspin-spyworks-part-4.md)
> [Successivo](tailspin-spyworks-part-6.md)
