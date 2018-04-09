---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: 'Introduzione a ASP.NET Web Pages: creazione di un Layout coerente | Documenti Microsoft'
author: tfitzmac
description: In questa esercitazione viene illustrato come utilizzare layout per creare un aspetto coerente per le pagine in un sito che utilizza le pagine Web ASP.NET. Si presuppone di aver completato il...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: c2d5c4d8ed8a71979c16d484ab90d283a45de537
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a>Introduzione a ASP.NET Web Pages - creazione di un Layout coerenza
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questa esercitazione viene illustrato come utilizzare *layout* per creare un aspetto coerente per le pagine in un sito che usa ASP.NET Web Pages. Si presuppone di aver completato la serie tramite [l'eliminazione di dati del Database in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).
> 
> Illustra quanto segue:
> 
> - È una pagina di layout.
> - Come combinare pagine di layout con contenuto dinamico.
> - Come passare valori a una pagina di layout.


## <a name="about-layouts"></a>Informazioni sui layout

Le pagine creati finora sono state tutte complete, pagine autonome. Appartengono allo stesso sito, ma non hanno un aspetto standard o eventuali elementi comuni.

La maggior parte dei siti dispongono di un aspetto coerente e layout. Ad esempio, se si passa al [Microsoft.com](https://www.microsoft.com/web/) del sito e di eseguire, vedrai che tutte le pagine siano conformi a un layout generale e a un tema di visual:

![Pagina del sito Microsoft.com del layout dell'intestazione, dell'area di navigazione, l'area del contenuto e i piè di pagina](layouts/_static/image1.png)

Un *inefficiente* per creare il layout, è possibile definire barra di spostamento, intestazione e piè di pagina separatamente in ciascuna delle pagine. È necessario duplicare gli stessi tag ogni volta che. Se si desidera modificare un elemento (ad esempio, aggiornare il piè di pagina), è necessario modificare separatamente ogni pagina.

Ovvero where *pagine di layout* sono disponibili in. In ASP.NET Web Pages, è possibile definire una pagina di layout che fornisce un contenitore complessivo di pagine del sito. Pagina di layout, ad esempio, può contenere l'intestazione dell'area di navigazione e nel piè di pagina. Pagina di layout include un segnaposto in cui passa il contenuto principale.

È quindi possibile definire singole pagine di contenuto che contengono il markup e il codice solo per tale pagina. Pagine di contenuto non è necessario disporre di pagine HTML complete; essi non è necessario avere un `<body>` elemento. Dispongono anche di una riga di codice che indica ad ASP.NET la pagina di layout che si desidera visualizzare il contenuto. Di seguito è riportata un'immagine che mostra circa il funzionamento di questa relazione:

![Diagramma concettuale che mostra due pagine di contenuto e una pagina di layout in cui lo spazio è sufficiente](layouts/_static/image2.png)

Questa interazione è facile da comprendere quando si visualizza l'azione. In questa esercitazione si modificherà le pagine di filmati per utilizzare un layout.

## <a name="adding-a-layout-page"></a>Aggiunta di una pagina di Layout

Si inizierà creando una pagina di layout che consente di definire un layout di pagina tipiche con un'intestazione e piè di pagina, un'area per il contenuto principale. Nel sito WebPagesMovies, aggiungere una pagina CSHTML denominata  *\_cshtml*.

Il carattere di sottolineatura iniziale ( `_` ) carattere è significativo. Se il nome della pagina inizia con un carattere di sottolineatura, ASP.NET non inviare direttamente la pagina nel browser. Questa convenzione consente di definire le pagine che sono necessari per il sito, ma gli utenti non devono essere in grado di richiedere direttamente.

Sostituire il contenuto della pagina con le operazioni seguenti:

[!code-html[Main](layouts/samples/sample1.html)]

Come si può notare, questo codice è appena HTML che utilizza `<div>` elementi per definire più di tre sezioni in una pagina più uno `<div>` elemento per contenere le tre sezioni. Il piè di pagina contiene una combinazione di codice Razor: `@DateTime.Now.Year`, che verrà eseguito il rendering dell'anno corrente in tale posizione nella pagina.

Si noti che è presente un collegamento a un foglio di stile denominato *Movies.css*. Il foglio di stile è in cui verranno definiti i dettagli del layout fisico degli elementi. Che verranno create in proposito.

Le uniche funzionalità in questa  *\_cshtml* pagina è la `@Render.Body()` riga. Che è un segnaposto in cui verrà salvato il contenuto quando il layout è unito a un'altra pagina.

## <a name="adding-a-css-file"></a>Aggiunta di un File con estensione CSS

Il modo migliore per definire la disposizione effettiva (ovvero, aspetto) di elementi nella pagina consiste nell'utilizzare le regole di fogli di stile. Quindi si creerà un *CSS* file con le regole per il nuovo layout.

In WebMatrix, selezionare la directory radice del sito. Quindi nel **file** scheda della barra multifunzione, fare clic sulla freccia sotto il **New** pulsante e quindi fare clic su **nuova cartella**.

![L'opzione 'Nuova cartella' in nuovi nella barra multifunzione.](layouts/_static/image3.png)

Denominare la nuova cartella *stili*.

![Denominazione nella nuova cartella 'Stili'](layouts/_static/image4.png)

All'interno di nuovo *stili* cartella, creare un file denominato *Movies.css*.

![Creazione di un nuovo file Movies.css](layouts/_static/image5.png)

Sostituire il contenuto del nuovo *CSS* file con le operazioni seguenti:

[!code-css[Main](layouts/samples/sample2.css)]

Non ci si riferisce a molte di queste regole CSS, tranne al notare due cose. Uno è che oltre a impostare i tipi di carattere e dimensioni, le regole di usare il posizionamento assoluto per stabilire il percorso di area del contenuto principale, intestazione e piè di pagina. Se si ha familiarità con posizionamento CSS, è possibile leggere il [posizionamento CSS](http://www.w3schools.com/css/css_positioning.asp) esercitazione nel sito di W3Schools.

L'altra cosa da notare è che, nella parte inferiore, è stato copiato le regole di stile che facevano originariamente definito singolarmente nel *Movies.cshtml* file. Queste regole sono state usate nel [Introduzione alla visualizzazione di dati da utilizzando ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) esercitazione per rendere il `WebGrid` helper il rendering del markup che alla tabella aggiunta vengono archiviati con striping. (Se prevede di utilizzare un *CSS* file per le definizioni di stile, è possibile inserire anche le regole di stile per l'intero sito in essa contenuti.)

## <a name="updating-the-movies-file-to-use-the-layout"></a>L'aggiornamento del File di filmati per usare il Layout

Ora è possibile aggiornare i file esistenti nel sito per utilizzare il nuovo layout. Aprire il *Movies.cshtml* file. Nella parte superiore, come la prima riga del codice, aggiungere quanto segue:

[!code-csharp[Main](layouts/samples/sample3.cs)]

L'ora verrà visualizzata una pagina in questo modo:

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

Questa riga di codice indica ad ASP.NET che, quando il *filmati* pagina è in esecuzione, deve essere unito con il  *\_cshtml* file.

Poiché il *Movies.cshtml* file utilizza ora una pagina di layout, è possibile rimuovere il markup del *Movies.cshtml* pagina che ha preso in considerazione per il  *\_cshtml*file. Estrarre il `<!DOCTYPE>`, `<html>`, e `<body>` apertura e chiusura. Estrarre l'intero `<head>` elemento e il relativo contenuto, che include le regole di stile per la griglia, poiché le regole ora sono disponibili un *CSS* file. Mentre si è il, modificare esistente `<h1>` elemento da un `<h2>` elemento; è un `<h1>` elemento nella pagina di layout è già. Modifica il `<h2>` testo "Elenco di film".

In genere non occorre effettuare questi tipi di modifiche in una pagina di contenuto. Quando si avvia il sito con una pagina di layout, è creare pagine di contenuto senza tutti questi elementi per iniziare. In questo caso, tuttavia, convertire una pagina autonoma in uno che utilizza un layout, pertanto non c'è un po' di pulizia.

Al termine, il *Movies.cshtml* pagina sarà simile al seguente:

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a>Test del Layout

Ora è possibile visualizzare il layout l'aspetto seguente. In WebMatrix, fare doppio clic su di *Movies.cshtml* pagina e selezionare **Avvia nel browser**. Quando il browser visualizza la pagina, l'aspetto è simile a questa pagina:

![Rendering utilizzando un layout della pagina filmati](layouts/_static/image6.png)

ASP.NET è incorporato il contenuto della pagina in Movies.cshtml il  *\_cshtml* pagina destra in cui il `RenderBody` metodo. E naturalmente il  *\_cshtml* pagina riferimenti un *CSS* file che definisce l'aspetto della pagina.

## <a name="updating-the-addmovie-page-to-use-the-layout"></a>Aggiornamento della pagina AddMovie per usare il Layout

Il vantaggio di layout è che è possibile usarli per tutte le pagine del sito. Aprire il *AddMovie.cshtml* pagina.

È possibile ricordare che il *AddMovie.cshtml* pagina era originariamente alcune regole CSS in per definire l'aspetto dei messaggi di errore di convalida. Dal momento che disponibile un *CSS* file per ora il sito, è possibile spostare tali regole per il *CSS* file. Rimuoverli dal *AddMovie.cshtml* file e li aggiunge alla fine del *Movies.css* file. Si siano spostando le regole seguenti:

[!code-css[Main](layouts/samples/sample6.css)]

Apportare la stessa natura delle modifiche in *AddMovie.cshtml* che hai usato *Movies.cshtml* , aggiungere `Layout="~/_Layout.cshtml;` e rimuovere il markup HTML che si trova ora estraneo. Modificare l'elemento `<h1>` in `<h2>`. Al termine, la pagina verrà aspetto simile al seguente:

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

Eseguire la pagina. Ora sembra che questa illustrazione:

![Rendering utilizzando un layout della pagina 'Aggiungi filmati'](layouts/_static/image7.png)

Si desidera apportare modifiche analoghe alle pagine del sito, ovvero *EditMovie.cshtml* e *DeleteMovie.cshtml*. Tuttavia, prima di procedere, è possibile apportare modifica un altro per il layout che rende più flessibile.

## <a name="passing-title-information-to-the-layout-page"></a>Passaggio di informazioni sul titolo di pagina di Layout

Il  *\_cshtml* pagina creato ha un `<title>` elemento che è impostato su "Sito film personale". La maggior parte dei browser di visualizzare il contenuto di questo elemento come il testo in una scheda:

![La pagina &lt;titolo&gt; elemento visualizzato in una scheda del browser](layouts/_static/image8.png)

Queste informazioni di titolo sono generiche. Si supponga che il testo del titolo in modo più specifico per la pagina corrente. (Il testo del titolo è utilizzato anche da motori di ricerca per determinare la pagina sulle.) È possibile passare le informazioni da una pagina di contenuto come *Movies.cshtml* o *AddMovie.cshtml* alla pagina di layout e quindi utilizzare tali informazioni per personalizzare i contenuti della pagina di layout, esegue il rendering.

Aprire il *Movies.cshtml* pagina nuovamente. Nel codice nella parte superiore, aggiungere la riga seguente:

[!code-csharp[Main](layouts/samples/sample8.cs)]

Il `Page` oggetto è disponibile in tutti *. cshtml* pagine ed è a questo scopo, in particolare per condividere informazioni tra una pagina e il layout.

Aprire il<em>\_cshtml</em> pagina. Modifica il `<title>` elemento in modo che questo markup:

[!code-html[Main](layouts/samples/sample9.html)]

Questo codice esegue il rendering di elementi di `Page.Title` proprietà direttamente in tale posizione nella pagina.

Eseguire il *Movies.cshtml* pagina. Questa volta la scheda del browser mostra ciò che è passato come valore di `Page.Title`:

![Mostra il titolo creato dinamicamente una scheda del browser](layouts/_static/image9.png)

Se si desidera, è possibile visualizzare l'origine della pagina nel browser. È possibile vedere che il `<title>` elemento sottoposto a rendering come `<title>List Movies</title>`.

> [!TIP] 
> 
> **Oggetto Page**
> 
> Una funzionalità utile dei `Page` è un oggetto dinamico, ovvero il `Title` proprietà non è un nome predefinito o riservato. È possibile utilizzare *qualsiasi* nome per il valore di `Page` oggetto. Ad esempio, si può facilmente aver passato il titolo utilizzando una proprietà denominata `Page.CurrentName` o `Page.MyPage`. L'unica restrizione è che il nome deve seguire le normali regole per le proprietà possono essere denominate. (Ad esempio, il nome non può contenere uno spazio).
> 
> È possibile passare qualsiasi numero di valori utilizzando il `Page` oggetto. Se si desidera passare le informazioni sui film alla pagina di layout, è possibile passare i valori con un risultato simile `Page.MovieTitle` e `Page.Genre` e `Page.MovieYear`. In alternativa, tutti gli altri nomi che inventati per archiviare le informazioni. L'unico requisito, ovvero probabilmente ovvio, è che è necessario utilizzare gli stessi nomi nella pagina di contenuto e la pagina di layout.
> 
> Le informazioni passate tramite il `Page` oggetto non è limitato solo testo da visualizzare nella pagina di layout. È possibile passare un valore per la pagina di layout e quindi il codice nella pagina layout possibile utilizzare il valore di decidere se visualizzare una sezione della pagina, quali *CSS* per l'utilizzo di file e così via. I valori passati di `Page` oggetto sono come qualsiasi altro valori che si utilizzano nel codice. È sufficiente che i valori da cui proviene la pagina di contenuto e vengono passati alla pagina di layout.


Aprire il *AddMovie.cshtml* pagina e aggiungere una riga all'inizio del codice che fornisce un titolo per il *AddMovie.cshtml* pagina:

[!code-csharp[Main](layouts/samples/sample10.cs)]

Eseguire il *AddMovie.cshtml* pagina. È visualizzato il nuovo titolo:

![Mostra il titolo 'Aggiungere filmati' creato dinamicamente una scheda del browser](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a>Aggiornare le pagine rimanenti per usare il Layout

A questo punto sarà possibile completare le pagine rimanenti del sito in modo che utilizzino il nuovo layout. Aprire *EditMovie.cshtml* e *DeleteMovie.cshtml* a sua volta e apportare le stesse modifiche in ognuno.

Aggiungere la riga di codice che si collega alla pagina di layout:

[!code-csharp[Main](layouts/samples/sample11.cs)]

Aggiungere una riga per impostare il titolo della pagina:

[!code-csharp[Main](layouts/samples/sample12.cs)]

oppure:

[!code-csharp[Main](layouts/samples/sample13.cs)]

Rimuovere il markup HTML estraneo, lasciare in pratica, solo i bit che si trovano all'interno di `<body>` elemento (e il blocco di codice nella parte superiore).

Modifica il `<h1>` elemento sia una `<h2>` elemento.

Quando sono state apportate queste modifiche, il test di ogni e assicurarsi che vengano visualizzate correttamente e che il titolo sia corretto.

## <a name="parting-thoughts-about-layout-pages"></a>Opinioni PROD143 sulle pagine di Layout

In questa esercitazione è stato creato un  *\_cshtml* pagina e utilizzata la `RenderBody` metodo per unire il contenuto da un'altra pagina. Che rappresenta il modello di base per l'utilizzo dei layout nelle pagine Web.

Pagine di layout dispongono di funzionalità aggiuntive che non sono stati trattati qui. Ad esempio, è possibile annidare le pagine di layout, ovvero una pagina di layout può a sua volta riferimento un'altra. Layout nidificati può essere utile se si lavora con le sezioni di un sito che richiedono i layout diversi. È inoltre possibile utilizzare i metodi aggiuntivi (ad esempio, `RenderSection`) per impostare denominato sezioni della pagina di layout.

La combinazione di pagine di layout e *CSS* file è molto efficace. Come si vedrà nella serie esercitazione successiva, in WebMatrix è possibile creare un sito basato su un *modello*, che consente di un sito che dispone di funzionalità predefinite in essa contenuti. I modelli di sfruttare le pagine di layout e CSS per creare siti che aspetto e che dispongono di funzionalità quali menu. Di seguito è riportata una schermata della home page del sito basato su un modello, mostrando le funzionalità che utilizzano pagine di layout e CSS:

![Layout creati dal modello di sito WebMatrix che mostra l'intestazione dell'area di navigazione, area di contenuto, sezione facoltativa e collegamenti di accesso](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a>Elenco completo per la pagina di film (aggiornata per l'utilizzo di una pagina di Layout)

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a>Pagina Completamento elenco per aggiungere la pagina di film (aggiornata per il Layout)

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a>Completare l'elenco di pagina per pagina film Delete (aggiornata per il Layout)

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a>Completare l'elenco di pagina per pagina in film modifica (aggiornata per il Layout)

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a>Esercitazione seguente

Nella prossima esercitazione, si apprenderà come pubblicare il sito Internet in modo che tutti gli utenti possono visualizzarlo.

## <a name="additional-resources"></a>Risorse aggiuntive

- [Creazione di un aspetto coerente](https://go.microsoft.com/fwlink/?LinkID=202891) , ovvero un articolo che fornisce alcune informazioni dettagliate sull'utilizzo dei layout. Viene inoltre descritto come passare un valore a una pagina di layout che mostra o nasconde la parte del contenuto.
- [Pagine di Layout con Razor annidati](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) , Mike Brind blog, un esempio di come annidare le pagine di layout. (Include il download delle pagine).

> [!div class="step-by-step"]
> [Precedente](deleting-data.md)
> [Successivo](publishing.md)
