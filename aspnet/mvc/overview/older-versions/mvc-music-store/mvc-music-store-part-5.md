---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: 'Parte 5: Modifica di moduli e modelli | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Parte 5 include moduli di modifica e creazione di modelli.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: c1065dcb45b6d28672edba32b95c7fc476c8b944
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838687"
---
<a name="part-5-edit-forms-and-templating"></a>Parte 5: Modifica di moduli e modelli
====================
da [Jon Galloway](https://github.com/jongalloway)

> Store la musica MVC è un'applicazione di esercitazione introduce e dettagliata spiega come usare ASP.NET MVC e Visual Studio per lo sviluppo web.  
>   
> La Store musica MVC è un'implementazione di archivio di esempio leggero che vende gli album musicali online e ne implementa amministrazione sito di base, accesso dell'utente e la funzionalità di carrello acquisti.
> 
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Parte 5 include moduli di modifica e creazione di modelli.


Nel capitolo precedente, abbiamo parlato il caricamento dei dati dal database e di visualizzarli. In questo capitolo è permetterà inoltre la modifica dei dati.

## <a name="creating-the-storemanagercontroller"></a>Creazione di StoreManagerController

Iniziamo creando un nuovo controller denominato **StoreManagerController**. Per questo controller, si sfrutteranno le funzionalità di Scaffolding disponibili in ASP.NET MVC 3 Tools Update. Impostare le opzioni per la finestra di dialogo Aggiungi Controller, come illustrato di seguito.

![](mvc-music-store-part-5/_static/image1.png)

Quando si fa clic sul pulsante Aggiungi, si noterà che il meccanismo di scaffolding di ASP.NET MVC 3 esegue una discreta quantità di lavoro per l'utente:

- Crea il nuovo StoreManagerController con una variabile locale di Entity Framework
- Aggiunge una cartella StoreManager cartella visualizzazioni del progetto
- È stata aggiunta la visualizzazione create. cshtml, cshtml, Details. cshtml, Edit. cshtml e index. cshtml, fortemente tipizzata per la classe Album

![](mvc-music-store-part-5/_static/image2.png)

La nuova classe controller StoreManager include CRUD (create, leggere, aggiornare ed eliminare) le azioni del controller che ha dimestichezza con Album di classe del modello e utilizzare il contesto di Entity Framework per l'accesso al database.

## <a name="modifying-a-scaffolded-view"></a>Modifica di una vista sottoposto a scaffolding

È importante ricordare che, sebbene questo codice è stato generato automaticamente, è codice standard di MVC ASP.NET, esattamente come è stato scritto in questa esercitazione. È destinato a risparmiare il tempo che sarebbero necessari sulla scrittura di codice del controller boilerplate e creazione manuale di visualizzazioni fortemente tipizzate, ma questo non è il tipo di codice generato si potrebbe aver notato preceduto avvertimenti nei commenti sul modo in cui è non deve modificare il codice. Si tratta del codice e vanno considerati per modificarlo.

Quindi, iniziamo con una rapida modifica alla vista StoreManager Index (/ Views/StoreManager/Index.cshtml). Questa vista visualizza una tabella che elenca gli album lo store con Modifica / dettagli / eliminare i collegamenti e include le proprietà pubbliche dell'Album. Si sarà rimuovere il campo AlbumArtUrl, perché non è molto utile in questa visualizzazione. Nelle &lt;tabella&gt; sezione del codice di visualizzazione, rimuovere il &lt;th&gt; e &lt;td&gt; elementi circostanti AlbumArtUrl riferimenti, come indicato dalle linee evidenziate seguente:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

Il codice di visualizzazione modificato avrà l'aspetto seguente:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a>Introduzione alla gestione di Store

Ora eseguire l'applicazione e passare a/StoreManager /. Verrà visualizzata appena modificata, che mostra un elenco di album nell'archivio con collegamenti di modifica, dettagli ed eliminare l'indice di Manager Store.

![](mvc-music-store-part-5/_static/image3.png)

Facendo clic sul collegamento Modifica consente di visualizzare un modulo di modifica con i campi dell'album, inclusi i menu a discesa per il genere e artista.

![](mvc-music-store-part-5/_static/image4.png)

Fare clic sul collegamento "Torna all'elenco" nella parte inferiore, quindi fare clic sul collegamento dei dettagli per un Album. Ciò consente di visualizzare le informazioni dettagliate per un Album singoli.

![](mvc-music-store-part-5/_static/image5.png)

Anche in questo caso, fare clic sul retro al collegamento di elenco, quindi fare clic su un collegamento di eliminazione. Verrà visualizzata una finestra di dialogo di conferma, che visualizza i dettagli di album e in cui viene chiesto se siamo sicuri che si desidera eliminarlo.

![](mvc-music-store-part-5/_static/image6.png)

Facendo clic sul pulsante Elimina nella parte inferiore sarà l'eliminazione dell'album e tornare alla pagina di indice, che mostra il album eliminato.

Non abbiamo ancora finito con la gestione di Store, ma abbiamo lavoro controller e visualizza il codice per le operazioni CRUD avviare da.

## <a name="looking-at-the-store-manager-controller-code"></a>Esaminando il codice del Controller di gestione di Store

Il Controller di gestione Store contiene una discreta quantità di codice. Verrà ora analizzato ciò dall'alto verso il basso. Il controller include alcuni spazi dei nomi standard per un controller MVC, nonché un riferimento a spazio dei nomi modelli. Il controller ha un'istanza privata di MusicStoreEntities, usati da ognuna delle azioni controller per l'accesso ai dati.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a>Store azioni Manager Index e Details

La visualizzazione dell'indice recupera un elenco di album, tra cui Genre e artista informazioni di riferimento ogni album, come abbiamo visto in precedenza quando si utilizza il metodo Store Sfoglia. La visualizzazione dell'indice segue i riferimenti agli oggetti collegati in modo che sia possibile visualizzare ogni album nome genere e nome dell'artista, in modo che il controller è in corso l'esecuzione di query per queste informazioni nella richiesta originale ed efficiente.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

Azione del controller i dettagli del StoreManager Controller funziona esattamente come l'azione di dettagli Controller Store è stata scritta in precedenza, viene eseguita una query per Album da ID usando il metodo Find (), lo restituisce quindi alla visualizzazione.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a>La creazione di metodi di azione

I metodi di azione Create sono leggermente diversi da quelle che abbiamo visto finora, in quanto essi gestiscono input del form. Quando un utente visita innanzitutto /StoreManager/creare/essi verrà visualizzati un form vuoto. Questa pagina HTML contiene un &lt;form&gt; elemento che contiene l'elenco a discesa e nella casella di testo di input gli elementi in cui è possibile immettere i dettagli dell'album.

Dopo che l'utente immette i valori di form Album, premere il pulsante "Salva" per rinviare queste modifiche all'applicazione per salvare nel database. Quando l'utente preme il pulsante "Salva" i &lt;form&gt; eseguirà un POST HTTP al /StoreManager/creare/URL e invia il &lt;form&gt; valori come parte del POST HTTP.

ASP.NET MVC consente di suddividere facilmente la logica di questi due scenari di chiamata di URL da ci permette di implementare due metodi di azione "Crea" separati all'interno di nostra classe StoreManagerController: uno per la gestione iniziale Sfoglia HTTP-GET per il /StoreManager/Create / URL e l'altro per gestire la richiesta HTTP-POST delle modifiche inviate.

### <a name="passing-information-to-a-view-using-viewbag"></a>Il passaggio di informazioni a una visualizzazione usando ViewBag

Si usa ViewBag in precedenza in questa esercitazione, ma non hai parlato molto di informazioni. ViewBag consente di passare informazioni alla vista senza usare un oggetto modello fortemente tipizzato. In questo caso, l'azione del controller modifica HTTP-GET deve passare entrambi un elenco di generi e gli artisti al form per popolare gli elenchi a discesa e il modo più semplice per farlo è per restituirle come elementi ViewBag.

ViewBag è un oggetto dinamico, il che significa che è possibile digitare ViewBag.Foo o ViewBag.YourNameHere senza scrivere codice per definire le proprietà. In questo caso, il codice del controller Usa ViewBag.GenreId e ViewBag.ArtistId in modo che i valori di elenco a discesa inviati con il modulo saranno GenreId e ArtistId, ovvero si prevede di impostare le proprietà di Album.

Questi valori di elenco a discesa vengono restituiti al modulo tramite l'oggetto SelectList, che viene compilato per tale scopo. Questa operazione viene eseguita usando codice simile al seguente:

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

Come può notare dal codice del metodo di azione, tre parametri vengono utilizzati per creare questo oggetto:

- L'elenco di elementi che venga visualizzato l'elenco a discesa. Si noti che non si tratta di una stringa viene passata un elenco dei generi.
- Il parametro successivo passato al SelectList è il valore selezionato. In questo modo il SelectList sa come pre-selezionare un elemento nell'elenco. Questo sarà più facile da comprendere quando si esamina il modulo di modifica, che è piuttosto simile.
- Il parametro finale è la proprietà da visualizzare. In questo caso, ciò indica che la proprietà Genre.Name è ciò che verrà visualizzato all'utente.

Con tali presupposti, quindi, l'azione HTTP-GET creare è piuttosto semplice: due SelectLists vengono aggiunti alla ViewBag e nessun oggetto del modello viene passato al modulo (dal momento che non è ancora stato creato).

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a>Helper HTML per visualizzare gli elenchi a rilasciare nell'istruzione Create View

Poiché abbiamo parlato di come l'elenco a discesa di valori vengono passati alla visualizzazione, diamo un'occhiata la visualizzazione per vedere come vengono visualizzati i valori. Nel codice di visualizzazione (/ Views/StoreManager/Create.cshtml), si noterà che viene effettuata la chiamata seguente per visualizzare il genere verso il basso.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

Questo è noto come un HTML Helper - un metodo di utilità che esegue un'attività comune di visualizzazione. Gli helper HTML sono molto utili nel mantenere il codice di visualizzazione concise e leggibili. L'helper Html.DropDownList viene fornito da ASP.NET MVC, ma come vedremo in seguito è possibile creare un helper per visualizzare il codice che verrà riutilizzato nell'applicazione.

La chiamata Html.DropDownList deve solo in due cose - dove per ottenere l'elenco per visualizzare e quale valore (se presente) deve essere già selezionato. Il primo parametro, GenreId, indica a DropDownList per individuare un valore denominato GenreId nel modello o ViewBag. Il secondo parametro viene utilizzato per indicare il valore da mostrare come inizialmente selezionato nell'elenco a discesa. Poiché questo modulo è un modulo di creazione, è presente alcun valore per essere preselezionate e String. Empty viene passato.

### <a name="handling-the-posted-form-values"></a>Gestisce i valori del modulo registrato

Come accennato prima, esistono due metodi di azione associati a ogni modulo. Il primo gestisce la richiesta HTTP GET e visualizza il form. La seconda gestisce la richiesta HTTP-POST, che contiene i valori di modulo inviato. Si noti che l'azione del controller ha un attributo [HttpPost], che indica a MVC ASP.NET che devono rispondere solo alle richieste HTTP POST.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

Questa azione ha quattro responsabilità:

- 1. Leggere i valori del modulo
- 2. Controllare se i valori del modulo passare qualsiasi regola di convalida
- 3. Se l'invio del modulo è valido, salvare i dati e visualizzare l'elenco aggiornato
- 4. Se l'invio del modulo non è valido, rivisualizzare il form con errori di convalida

#### <a name="reading-form-values-with-model-binding"></a>Lettura di valori di Form con associazione di modelli

L'azione del controller elabora l'invio di un form che include i valori per GenreId e ArtistId (dall'elenco a discesa) e i valori nella casella di testo per titolo, prezzo e AlbumArtUrl. Sebbene sia possibile accedere direttamente ai valori del form, un approccio migliore consiste nell'utilizzare le funzionalità di associazione di modelli incorporate in ASP.NET MVC. Quando un'azione del controller richiede un tipo di modello come parametro, ASP.NET MVC tenterà di popolare un oggetto di quel tipo usando gli input dei moduli (nonché i valori di route e stringa di query). A tale scopo, alla ricerca di valori i cui nomi corrispondano alle proprietà dell'oggetto modello, ad esempio, quando si imposta il nuovo Album valore dell'oggetto GenreId, Cerca un input con il nome GenreId. Quando si creano visualizzazioni con i metodi standard di ASP.NET MVC, il form verrà sempre rappresentato usando nomi di proprietà come nomi di campo di input, in modo che i nomi dei campi verrà semplicemente trovata corrispondenza.

#### <a name="validating-the-model"></a>La convalida del modello

Il modello viene convalidato con una semplice chiamata di ModelState. Qualsiasi regola di convalida è ancora stato aggiunto alla nostra classe Album ancora - è possibile farlo tra poco: a questo punto questo controllo non ha molto da fare. È importante che questa verifica ModelStat.IsValid adatterà alle regole di convalida che sono state inserite nel nostro modello, in modo che le modifiche future alle regole di convalida non richiedano la presenza di aggiornamenti di codice di azione del controller.

#### <a name="saving-the-submitted-values"></a>Salvare i valori inviati

Se l'invio del modulo supera la convalida, è possibile salvare i valori nel database. Con Entity Framework, che richiede solo l'aggiunta del modello alla raccolta di album e la chiamata a SaveChanges.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

Entity Framework genera i comandi SQL appropriati per rendere persistente il valore. Dopo aver salvato i dati, Microsoft reindirizza tornare all'elenco di album quindi, vediamo l'aggiornamento. Questa operazione viene eseguita restituendo RedirectToAction con il nome dell'azione del controller a cui che si desidera visualizzare. In questo caso, si tratta del metodo di indice.

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a>Visualizzazione di invii di form non è valido con errori di convalida

Nel caso di input del modulo non è valido, i valori di elenco a discesa vengono aggiunti a ViewBag (come nel caso di HTTP GET) e vengono passati i valori del modello associato alla visualizzazione per la visualizzazione. Errori di convalida vengono visualizzati automaticamente utilizzando il @Html.ValidationMessageFor HTML Helper.

#### <a name="testing-the-create-form"></a>Il modulo di creazione di test

Per testare questo, Esegui l'applicazione e passare a /StoreManager/creare / - verrà visualizzato il form vuoto che è stato restituito dal metodo StoreController creare HTTP-GET.

Inserire alcuni valori e fare clic sul pulsante Crea per inviare il modulo.

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a>Gestione delle modifiche

La coppia di azione di modifica (HTTP-GET e HTTP-POST) è molto simile ai metodi di azione Create che abbiamo appena visto. Poiché lo scenario di modifica implica l'utilizzo di un album esistente, la modifica HTTP-GET metodo carica dell'Album in base al parametro "id", passato tramite la route. Questo codice per il recupero di un album da AlbumId è lo stesso come in precedenza abbiamo esaminato in azione del controller i dettagli. Come con la creazione / HTTP-GET (metodo), nell'elenco a discesa di valori vengono restituiti tramite ViewBag. Ciò consente di restituire un Album come l'oggetto modello per la visualizzazione (che è fortemente tipizzata per la classe di Album) durante il passaggio di dati aggiuntivi (ad esempio, un elenco dei generi) tramite ViewBag.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

L'azione di modifica HTTP-POST è molto simile all'azione di creazione HTTP-POST. L'unica differenza è che invece di aggiungere un nuovo album al database. Insieme di album, abbiamo stiamo ricerca l'istanza corrente dell'Album usando db. Entry(album) e impostando il relativo stato su Modified. Indica a Entity Framework che viene modificata un album esistente anziché crearne uno nuovo.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

È possibile testare questa orizzontale che esegue l'applicazione e passare a/StoreManger/e quindi scegliendo il collegamento di modifica per un album.

![](mvc-music-store-part-5/_static/image9.png)

Ciò consente di visualizzare il modulo di modifica illustrato dal metodo Edit HTTP-GET. Inserire alcuni valori e fare clic sul pulsante Salva.

![](mvc-music-store-part-5/_static/image10.png)

Si invia il form, Salva i valori e Stati Uniti restituisce all'elenco di Album, che mostra che i valori sono stati aggiornati.

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a>Gestisce l'eliminazione

L'eliminazione segue lo stesso modello come creazione, usando l'azione di un controller per visualizzare il form di conferma e un'altra azione del controller per gestire l'invio del modulo e modifica.

L'azione del controller Delete HTTP-GET equivale esattamente l'azione del controller Store Manager dettagli precedenti.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

Viene visualizzato un form che deve essere fortemente tipizzato a un tipo di Album, usando il modello di contenuto visualizzazione Delete.

![](mvc-music-store-part-5/_static/image12.png)

Il modello di eliminazione Mostra tutti i campi per il modello, ma possiamo semplificare tale down abbastanza. Modificare la visualizzazione del codice /Views/StoreManager/Delete.cshtml come segue.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

Verrà visualizzato un messaggio di conferma Delete semplificata.

![](mvc-music-store-part-5/_static/image13.png)

Facendo clic sul pulsante Elimina fa sì che il modulo per eseguire il postback al server che esegue l'azione DeleteConfirmed.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

L'azione del Controller Delete HTTP-POST esegue le azioni seguenti:

- 1. Carica dell'Album da ID
- 2. Elimina dell'album e salvare le modifiche
- 3. Effettua il reindirizzamento all'indice, che mostra che Album è stato rimosso dall'elenco

Per verificarlo, eseguire l'applicazione e passare a /StoreManager. Selezionare un album dall'elenco e fare clic sul collegamento di eliminazione.

![](mvc-music-store-part-5/_static/image14.png)

Verrà visualizzata la schermata di conferma Delete.

![](mvc-music-store-part-5/_static/image15.png)

Facendo clic sul pulsante di eliminazione rimuove album e ci riporta alla pagina di indice di Manager Store, che mostra che è stato eliminato dell'album.

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a>Uso di un HTML Helper personalizzati per troncare il testo

Si è verificato un problema con la pagina di indice Manager Store. Proprietà titolo Album e artista nome possono essere entrambi sufficientemente lunga che potrebbe generano disattivata la formattazione della tabella. Si creerà un HTML Helper personalizzati per consentire di troncare facilmente queste e altre proprietà nelle viste.

![](mvc-music-store-part-5/_static/image17.png)

Del Razor @helper sintassi ha reso molto semplice creare funzioni helper per l'uso nelle viste. Aprire la visualizzazione /Views/StoreManager/Index.cshtml e aggiungere il codice seguente direttamente dopo il @model riga.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

Questo metodo helper accetta una stringa e la lunghezza massima per consentire. Se il testo fornito è minore della lunghezza specificata, l'helper lo restituisce come-è. Se è più lungo, quindi tronca il testo e viene eseguito il rendering "..." per il resto.

A questo punto è possibile usare l'helper di troncamento per garantire che il titolo Album e artista il nome proprietà sono meno di 25 caratteri. Il codice di visualizzazione completa usando il nuovo helper Truncate viene visualizzata sotto.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

A questo punto quando si passa l'URL /StoreManager/, di album e i titoli vengono mantenuti sotto la lunghezza massima.

![](mvc-music-store-part-5/_static/image18.png)

Nota: Viene illustrato il caso di creazione e uso di un helper in un'unica visualizzazione semplice. Per altre informazioni sulla creazione di helper che è possibile usare nell'intero sito, vedere il blog post: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)


> [!div class="step-by-step"]
> [Precedente](mvc-music-store-part-4.md)
> [Successivo](mvc-music-store-part-6.md)
