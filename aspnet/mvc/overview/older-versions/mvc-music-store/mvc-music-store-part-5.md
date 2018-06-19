---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: 'Parte 5: Moduli di modifica e i modelli | Documenti Microsoft'
author: jongalloway
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio. Parte 5 vengono illustrati i moduli di modifica e modelli.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: d584e614b5a4124044cd9decd2272192ca164643
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874912"
---
<a name="part-5-edit-forms-and-templating"></a>Parte 5: Modifica moduli e modelli
====================
da [Jon Galloway](https://github.com/jongalloway)

> L'archivio di musica MVC è un'applicazione di esercitazione che vengono presentati e dettagliate sull'utilizzo di MVC ASP.NET e Visual Studio per lo sviluppo web.  
>   
> L'archivio di musica MVC è un'implementazione dell'archivio di esempio semplice che vende album musicali online e ne implementa amministrazione sito di base, account utente e funzionalità di carrello acquisti.
> 
> Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio. Parte 5 vengono illustrati i moduli di modifica e modelli.


Nel capitolo precedente, è durante il caricamento dei dati dal database e visualizzarlo. In questo capitolo, si verrà abilitata la modifica dei dati.

## <a name="creating-the-storemanagercontroller"></a>Creazione di StoreManagerController

Iniziare creando un nuovo controller chiamato **StoreManagerController**. Per questo controller, si verrà essere sfruttano le funzionalità di Scaffolding disponibili in ASP.NET MVC 3 Tools Update. Impostare le opzioni per la finestra di dialogo Aggiungi Controller, come illustrato di seguito.

![](mvc-music-store-part-5/_static/image1.png)

Quando si fa clic sul pulsante Aggiungi, si noterà che il meccanismo di scaffolding di ASP.NET MVC 3 non è una discreta quantità di lavoro per l'utente:

- Crea il nuovo StoreManagerController con una variabile locale di Entity Framework
- Aggiunge una cartella StoreManager cartella visualizzazioni del progetto
- Aggiunge Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml e cshtml visualizzazione fortemente tipizzata per la classe Album

![](mvc-music-store-part-5/_static/image2.png)

La nuova classe controller StoreManager include CRUD (create, leggere, aggiornare ed eliminare) le azioni del controller che sono in grado di utilizzare Album classe del modello e utilizzare il contesto di Entity Framework per l'accesso al database.

## <a name="modifying-a-scaffolded-view"></a>Modifica di una vista scaffolding

È importante tenere presente che, sebbene questo codice è stato generato automaticamente, è codice standard di ASP.NET MVC, esattamente come è stato della scrittura in questa esercitazione. È progettato per risparmiare il tempo che sarebbero necessari sulla scrittura di codice boilerplate del controller e la creazione di visualizzazioni fortemente tipizzate manualmente, ma non il tipo di codice generato si potrebbe aver notato preceduto da avvertimenti nei commenti sulle non deve cambiare la codice. Questo è il codice e previsto per modificarlo.

In tal caso, iniziamo con una modifica rapida per la visualizzazione dell'indice StoreManager (/ Views/StoreManager/Index.cshtml). Questa vista viene visualizzata una tabella che elenca gli album Negozio con Modifica / dettagli / eliminare collegamenti e include le proprietà pubbliche dell'Album. Il campo AlbumArtUrl, rimuoverò perché non è molto utile in questa visualizzazione. In &lt;tabella&gt; sezione del codice di visualizzazione, rimuovere il &lt;th&gt; e &lt;td&gt; elementi circostanti AlbumArtUrl riferimenti, come indicato dalle righe evidenziate riportato di seguito:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

Il codice di visualizzazione modificato verrà visualizzato come segue:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a>Un primo sguardo al gestore del Negozio

Esegui l'applicazione e passare a/StoreManager /. Consente di visualizzare l'indice di gestione archiviazione appena modificata, che mostra un elenco di quelli nell'archivio con collegamenti a informazioni dettagliate, modifica ed Elimina.

![](mvc-music-store-part-5/_static/image3.png)

Il collegamento di modifica un modulo di modifica con i campi per visualizzare Album, inclusi elenchi a discesa per Genre e artista.

![](mvc-music-store-part-5/_static/image4.png)

Fare clic sul collegamento "Torna all'elenco" nella parte inferiore, quindi fare clic sul collegamento dettagli per un Album. Consente di visualizzare le informazioni dettagliate per un singolo Album.

![](mvc-music-store-part-5/_static/image5.png)

Nuovamente, fare clic su nella parte posteriore al collegamento di elenco, quindi fare clic sul collegamento Elimina. Verrà visualizzata una finestra di dialogo di conferma, che mostra i dettagli dell'album e viene chiesto se si è certi di voler eliminare.

![](mvc-music-store-part-5/_static/image6.png)

Fare clic sul pulsante Elimina nella parte inferiore verrà eliminare album e tornare alla pagina di indice, che mostra album eliminato.

Non è stata completata con la gestione di Store, ma non è disponibile l'utilizzo di controller e visualizza il codice per le operazioni CRUD avviare da.

## <a name="looking-at-the-store-manager-controller-code"></a>Esaminando il codice del Controller di gestione di Store

Il Controller di gestione di Store contiene una discreta quantità di codice. Verrà ora analizzato questo dall'alto verso il basso. Il controller include alcuni spazi dei nomi standard per un controller MVC, nonché un riferimento a questo spazio dei nomi modelli. Il controller ha un'istanza privata della MusicStoreEntities, ognuna delle azioni controller utilizzato per accedere ai dati.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a>Registrare azioni dell'indice di gestione e i dettagli

La visualizzazione dell'indice recupera un elenco di album, tra cui Genre e artista informazioni di riferimento ogni album, come illustrato in precedenza quando si utilizza il metodo Store Sfoglia. La visualizzazione dell'indice è dopo i riferimenti a oggetti collegati in modo da poter visualizzare ogni album Genre e artista nome, in modo efficiente e l'esecuzione di query per ottenere questa informazione nella richiesta originale, è in corso il controller.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

Azione del controller i dettagli del StoreManager Controller funziona esattamente come l'azione di dettagli sui Controller di archiviazione è scritto in precedenza, viene eseguita una query per Album dall'ID utilizzando il metodo Find(), lo restituisce quindi alla visualizzazione.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a>La creazione di metodi di azione

I metodi di azione Create sono leggermente diversi rispetto a quella che abbiamo visto finora, perché gestiscono l'input del form. Quando un utente visita prima /StoreManager/creazione/essi verrà visualizzati un form vuoto. La pagina HTML conterrà un &lt;modulo&gt; elemento che contiene l'elenco a discesa e casella di testo in cui possono inserire i dettagli dell'album di elementi di input.

Dopo che l'utente immette i valori di form Album, è possibile fare clic sul pulsante "Salva" per inviare che le modifiche all'applicazione per salvare nel database. Quando l'utente preme il pulsante "Salva" il &lt;modulo&gt; eseguirà un POST HTTP al /StoreManager/creazione/URL e invia il &lt;modulo&gt; valori come parte di HTTP-POST.

ASP.NET MVC consente di dividere costituisce la logica di questi due scenari di chiamata URL consentono di implementare due metodi di azione "Crea" distinti all'interno di classe StoreManagerController: gestire Sfoglia HTTP-GET iniziale per il /StoreManager/Create / URL e l'altra di gestire HTTP-POST delle modifiche inviate.

### <a name="passing-information-to-a-view-using-viewbag"></a>Passaggio di informazioni in una vista utilizzando ViewBag

È stato usato ViewBag precedentemente in questa esercitazione, ma non hai parlato molto su di esso. ViewBag consente di passare alla visualizzazione informazioni senza utilizzare un oggetto modello fortemente tipizzato. In questo caso, l'azione del controller modifica HTTP-GET deve passare un elenco di generi e artisti di modulo per popolare i menu a discesa e il modo più semplice per eseguire questa operazione è per restituirli come elementi ViewBag.

ViewBag è un oggetto dinamico, vale a dire che è possibile digitare ViewBag.Foo o ViewBag.YourNameHere senza scrivere codice per definire tali proprietà. In questo caso, il codice del controller utilizza ViewBag.GenreId e ViewBag.ArtistId in modo che i valori di elenco a discesa inviati con il modulo GenreId e ArtistId, ovvero si prevede di impostare le proprietà di Album.

I valori di elenco a discesa vengono restituiti al form utilizzando l'oggetto SelectList, che viene compilato solo a tale scopo. Questa operazione viene eseguita utilizzando codice simile al seguente:

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

Come si può vedere dal codice del metodo di azione, tre parametri vengono utilizzati per creare questo oggetto:

- L'elenco di elementi che prevede di visualizzare l'elenco a discesa. Si noti che questo non è una stringa, passaggio di un elenco di generi.
- Il parametro successivo da passare il SelectList è il valore selezionato. In questo modo il SelectList sia in grado di pre-selezionare un elemento nell'elenco. Questo sarà più facile da comprendere quando si esamina il modulo di modifica, è piuttosto simile.
- Il parametro finale è la proprietà da visualizzare. In questo caso, ciò indica che la proprietà Genre.Name è ciò che verrà visualizzato all'utente.

Tenendo presente, quindi, l'azione Create HTTP-GET è piuttosto semplice - due SelectLists vengono aggiunti al ViewBag e nessun oggetto modello viene passato al form (dal momento che non è stato ancora creato).

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a>Helper HTML per visualizzare il Drop down nell'istruzione Create View

Poiché sono stati trattati come elenco a discesa di valori vengono passati alla visualizzazione, è opportuno un rapido controllo di visualizzazione per vedere come vengono visualizzati i valori. Nel codice di visualizzazione (/ Views/StoreManager/Create.cshtml), si noterà che viene effettuata la chiamata seguente per visualizzare il genere verso il basso.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

Questo è noto come un HTML Helper, un metodo di utilità che consente di eseguire un'attività comune di visualizzazione. Helper HTML sono molto utili per mantenere il codice visualizza concise e leggibili. L'helper Html.DropDownList viene fornito da ASP.NET MVC, ma come vedremo in un secondo momento è possibile creare un helper per il codice di visualizzazione che verrà riutilizzato nell'applicazione.

La chiamata di Html.DropDownList deve semplicemente indicato due cose - in di ottenere l'elenco per visualizzare e valore (se presente) deve essere preselezionato. Il primo parametro, GenreId, indica DropDownList per cercare un valore denominato GenreId nel modello o ViewBag. Il secondo parametro è utilizzato per indicare il valore per visualizzare inizialmente selezionato nell'elenco a discesa. Poiché questo modulo è un modulo di creazione, è presente alcun valore per essere preselezionate e viene passato String. Empty.

### <a name="handling-the-posted-form-values"></a>La gestione dei valori di modulo registrato

Come accennato prima, esistono due metodi di azione associati a ogni modulo. Il primo gestisce la richiesta HTTP GET e visualizza il form. Il secondo gestisce la richiesta HTTP POST, che contiene i valori del modulo inviato. Si noti che l'azione del controller è un attributo [HttpPost], che indica ad ASP.NET MVC che deve rispondere solo alle richieste HTTP POST.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

Questa azione ha quattro responsabilità:

- 1. Leggere i valori del form
- 2. Controllare se i valori del form passare qualsiasi regola di convalida
- 3. Se l'invio del modulo è valido, salvare i dati e visualizzare l'elenco aggiornato
- 4. Se l'invio del modulo non è valido, rivisualizzare il form con errori di convalida

#### <a name="reading-form-values-with-model-binding"></a>Lettura di valori di Form con associazione di modelli

Invio di un form che contiene i valori per GenreId e ArtistId (dall'elenco a discesa) e i valori di casella di testo per titolo, prezzo e AlbumArtUrl elaborazione azione del controller. Sebbene sia possibile accedere direttamente ai valori del form, un approccio migliore è di utilizzare le funzionalità di associazione di modelli integrate in MVC ASP.NET. Quando un'azione del controller richiede un tipo di modello come parametro, verrà effettuato un tentativo di popolare un oggetto di quel tipo utilizzando formano l'input (così come i valori di route e querystring) ASP.NET MVC. A tale scopo, cercando valori i cui nomi corrispondano alle proprietà dell'oggetto modello, ad esempio, quando si imposta il nuovo Album valore dell'oggetto GenreId, la ricerca di un input con il nome GenreId. Quando si creano visualizzazioni utilizzando i metodi standard in ASP.NET MVC, il form verranno visualizzati sempre nomi delle proprietà come nomi di campo di input, in modo i nomi dei campi verrà semplicemente trovata corrispondenza.

#### <a name="validating-the-model"></a>La convalida del modello

Il modello viene convalidato con una semplice chiamata a ModelState.IsValid. È ancora stato aggiunto a qualsiasi regola di convalida per la classe Album ancora - faremo contenenti un bit - momento questo controllo non è possibile eseguire. L'aspetto importante è che questo controllo ModelStat.IsValid adatterà alle regole di convalida che è stato inserito nel modello in esame, pertanto le future modifiche alle regole di convalida non richiedono aggiornamenti per il codice di azione del controller.

#### <a name="saving-the-submitted-values"></a>Il salvataggio dei valori inviati

Se l'invio del modulo passa la convalida, è necessario salvare i valori nel database. Con Entity Framework, che richiede semplicemente aggiungere il modello di raccolta album e chiamare SaveChanges.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

Entity Framework genera comandi SQL corretti per mantenere il valore. Dopo aver salvato i dati, si reindirizza l'elenco di album come possiamo vedere l'aggiornamento. Questa operazione viene eseguita tramite la restituzione di RedirectToAction con il nome dell'azione del controller a cui che si desidera visualizzare. In questo caso, si tratta del metodo di indice.

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a>Visualizzazione di invii di form non valido con errori di convalida

Nel caso di input del form non valido, i valori di elenco a discesa vengono aggiunti a ViewBag (come nel caso di HTTP GET) e vengono passati i valori del modello associato alla visualizzazione per la visualizzazione. Errori di convalida vengono visualizzati automaticamente utilizzando il @Html.ValidationMessageFor HTML Helper.

#### <a name="testing-the-create-form"></a>Modulo di creazione di test

Per testare questo, Esegui l'applicazione e passare al /StoreManager/creazione / - verranno visualizzati è un modulo vuoto in cui è stato restituito dal metodo StoreController creare HTTP-GET.

Specificare alcuni valori e fare clic sul pulsante Crea per inviare il form.

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a>Gestione delle modifiche

La coppia di azione di modifica (HTTP-GET e HTTP-POST) è molto simile ai metodi di azione Crea che abbiamo appena esaminato. Poiché lo scenario di modifica implica l'utilizzo di un album esistente, la modifica HTTP-GET metodo carica Album in base al parametro "id", passato tramite la route. Questo codice per il recupero di un album di payload è lo stesso come in precedenza abbiamo esaminato nell'azione del controller di dettagli. Come con la creazione / HTTP-GET (metodo), nell'elenco a discesa di valori vengono restituiti tramite ViewBag. Ciò consente di restituire un Album come l'oggetto modello per la visualizzazione (che deve essere fortemente tipizzata per la classe di Album) durante il passaggio di dati aggiuntivi (ad esempio, un elenco di generi) tramite ViewBag.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

L'azione di modifica HTTP-POST è molto simile all'azione di creazione HTTP-POST. L'unica differenza è che, anziché aggiungere un nuovo album al database. Raccolta album, ci stiamo ricerca l'istanza corrente dell'Album tramite db. Entry(album) e impostare lo stato modificato. Ciò indica a Entity Framework che si sta modificando un album esistente anziché crearne uno nuovo.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

È possibile testarlo in esecuzione l'applicazione e la selezione di StoreManger/e scegliendo il collegamento di modifica di un album.

![](mvc-music-store-part-5/_static/image9.png)

Consente di visualizzare il modulo di modifica indicato dal metodo di modifica HTTP-GET. Specificare alcuni valori e fare clic sul pulsante Salva.

![](mvc-music-store-part-5/_static/image10.png)

Invia il form, Salva i valori e restituisce ci all'elenco di Album, che mostra che i valori siano stati aggiornati.

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a>Eliminazione di gestione

L'eliminazione segue dello stesso modello di modifica e creare, tramite l'azione di un controller per visualizzare il modulo di conferma e un'altra azione del controller per gestire l'invio del modulo.

Azione del controller HTTP-GET Delete corrisponde esattamente come l'azione del controller precedente dettagli di gestione di Store.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

A un tipo di Album, utilizzando il modello di contenuto di visualizzazione di eliminazione, viene visualizzato un form che deve essere fortemente tipizzato.

![](mvc-music-store-part-5/_static/image12.png)

Il modello di eliminazione Mostra tutti i campi per il modello, ma è possibile semplificare abbastanza tale verso il basso. Modificare la visualizzazione del codice /Views/StoreManager/Delete.cshtml al seguente.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

Verrà visualizzato un messaggio di conferma eliminazione semplificata.

![](mvc-music-store-part-5/_static/image13.png)

Facendo clic sul pulsante Elimina, il modulo eseguire il postback al server, che esegue l'azione DeleteConfirmed.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

L'azione del Controller Delete HTTP-POST esegue le azioni seguenti:

- 1. Carica Album da ID
- 2. Elimina album e salvare le modifiche
- 3. Effettua il reindirizzamento all'indice, che mostra che Album è stato rimosso dall'elenco

Per eseguire questa verifica, eseguire l'applicazione e passare a /StoreManager. Selezionare un album dall'elenco e fare clic sul collegamento Elimina.

![](mvc-music-store-part-5/_static/image14.png)

Verrà visualizzata la schermata di conferma eliminazione.

![](mvc-music-store-part-5/_static/image15.png)

Fare clic sul pulsante Delete rimuove album e noi restituisce alla pagina di indice di gestione di Store, indicante che è stato eliminato album.

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a>Utilizzo di un HTML Helper personalizzati per troncare il testo

Abbiamo un potenziale problema con la pagina di indice di gestione di Store. La proprietà titolo dell'Album e artista possono essere entrambi sufficientemente lunga che potrebbe potenzialmente generare disattivata la formattazione della tabella. Si creerà un HTML Helper personalizzati per consentire di troncare facilmente queste e altre proprietà nel nostro viste.

![](mvc-music-store-part-5/_static/image17.png)

Del Razor @helper sintassi ha reso piuttosto semplice creare funzioni di supporto per l'utilizzo nelle visualizzazioni. Aprire la vista /Views/StoreManager/Index.cshtml e aggiungere il codice seguente immediatamente dopo il @model riga.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

Questo metodo di supporto accetta una stringa e la lunghezza massima per consentire. Se il testo specificato è inferiore alla lunghezza specificata, l'helper genera come-è. Se più lungo, quindi tronca il testo e viene eseguito il rendering "…" per il resto.

Ora è possibile utilizzare l'helper di troncamento per assicurarsi che il titolo dell'Album e il nome artista proprietà sono meno di 25 caratteri. Il codice di visualizzazione completa utilizzando l'helper di troncamento nuovo viene visualizzato sotto.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

A questo punto quando si passa l'URL /StoreManager/, vengono mantenuti sotto la lunghezza massima di album e titoli.

![](mvc-music-store-part-5/_static/image18.png)

Nota: Viene illustrato il caso di creazione e utilizzo di un helper in una visualizzazione semplice. Per ulteriori informazioni sulla creazione di helper che è possibile utilizzare in tutto il sito, vedere il blog post: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)


> [!div class="step-by-step"]
> [Precedente](mvc-music-store-part-4.md)
> [Successivo](mvc-music-store-part-6.md)
