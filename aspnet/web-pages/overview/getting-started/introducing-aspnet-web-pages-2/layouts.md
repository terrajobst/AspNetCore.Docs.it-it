---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: 'Introduzione a ASP.NET Web Pages: creazione di un Layout coerente | Microsoft Docs'
author: tfitzmac
description: Questa esercitazione illustra come usare i layout per creare un aspetto coerente per le pagine in un sito che usa ASP.NET Web Pages. Si presuppone di aver completato il...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: 3368a3e3c9dc56b98ca0adddf4ffb181c7b34863
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379447"
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a>Introduzione a pagine Web ASP.NET - creazione di un Layout coerente
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questa esercitazione illustra come usare *layout* per creare un aspetto coerente per le pagine in un sito che usa ASP.NET Web Pages. Si presuppone di aver completato la serie attraverso [l'eliminazione dei dati di un Database in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).
> 
> Che cosa si apprenderà come:
> 
> - È una pagina di layout.
> - Come combinare le pagine di layout con contenuto dinamico.
> - Come passare valori a una pagina di layout.


## <a name="about-layouts"></a>Informazioni sui layout

Le pagine create finora sono stati tutti complete, pagine autonome. Appartengono allo stesso sito, ma non hanno un aspetto standard o tutti gli elementi comuni.

La maggior parte dei siti hanno un aspetto coerente e layout. Ad esempio, se si passa ad il [Microsoft.com/web](https://www.microsoft.com/web/) del sito e Guardatevi intorno, noterete che tutte le pagine siano conformi a un layout generale e a un tema di visual:

![Pagina del sito Web Microsoft.com/ che mostra il layout dell'intestazione, dell'area di navigazione, area di contenuto e il piè di pagina](layouts/_static/image1.png)

Un' *inefficiente* per creare questo layout sarebbe definire un'intestazione, barra di spostamento e il piè di pagina separatamente in ognuna delle pagine. È necessario duplicare lo stesso markup ogni volta. Se si desidera apportare delle modifiche (ad esempio, aggiornare il piè di pagina), sarebbe necessario modificare separatamente ogni pagina.

In questi casi *pagine di layout* sono disponibili in. In ASP.NET Web Pages, è possibile definire una pagina di layout che fornisce un contenitore complessivo per le pagine nel sito. Pagina di layout, ad esempio, può contenere l'intestazione dell'area di navigazione e nel piè di pagina. La pagina di layout include un segnaposto in cui passa il contenuto principale.

È quindi possibile definire singole pagine di contenuto che contengono il markup e il codice solo per tale pagina. Le pagine di contenuto non sono necessario essere pagine HTML complete; non ancora devono avere un `<body>` elemento. Hanno anche una riga di codice che indica quale pagina di layout che si desidera visualizzare il contenuto di ASP.NET. Ecco un'immagine che mostra all'incirca il funzionamento di questa relazione:

![Diagramma concettuale che mostra due pagine di contenuto e una pagina di layout in cui si adatta](layouts/_static/image2.png)

Questa interazione è facile da comprendere quando vederla in azione. In questa esercitazione, sarà necessario modificare le pagine di film per usare un layout.

## <a name="adding-a-layout-page"></a>Aggiunta di una pagina Layout

Inizierà creando una pagina di layout che consente di definire un layout di pagina tipiche con un'intestazione, piè di pagina e un'area per il contenuto principale. Nel sito WebPagesMovies, aggiungere una pagina con estensione CSHTML denominata  *\_layout. cshtml*.

Il carattere di sottolineatura iniziale ( `_` ) carattere è significativo. Se il nome di una pagina inizia con un carattere di sottolineatura, ASP.NET non invierà direttamente tale pagina nel browser. Questa convenzione consente di definire le pagine che sono necessarie per il sito, ma che gli utenti non devono essere in grado di richiedere direttamente.

Sostituire il contenuto della pagina con il codice seguente:

[!code-html[Main](layouts/samples/sample1.html)]

Come si può notare, questo markup è semplicemente codice HTML di cui viene utilizzata `<div>` elementi per definire più di tre sezioni nella pagina più uno `<div>` elemento per contenere le tre sezioni. Il piè di pagina contiene un frammento di codice Razor: `@DateTime.Now.Year`, che verrà eseguito il rendering dell'anno corrente nel percorso specificato nella pagina.

Si noti che è presente un collegamento a un foglio di stile denominato *Movies.css*. Il foglio di stile è dove verranno definiti i dettagli del layout fisico degli elementi. Che verrà creata più avanti.

La funzionalità uniche in questo  *\_layout. cshtml* pagina è il `@Render.Body()` riga. Che è un segnaposto dove verrà salvato il contenuto quando questo layout è unito a un'altra pagina.

## <a name="adding-a-css-file"></a>Aggiunta di un File con estensione CSS

Il modo migliore per definire la disposizione effettiva (vale a dire, aspetto) degli elementi nella pagina è usare le regole di fogli di stile. Quindi si creerà una *CSS* file con le regole per il nuovo layout.

In WebMatrix, selezionare la radice del sito. Quindi nel **file** della scheda della barra multifunzione, fare clic sulla freccia sotto il **New** pulsante e quindi fare clic su **nuova cartella**.

![L'opzione "Nuova cartella" in nuovi nella barra multifunzione.](layouts/_static/image3.png)

Denominare la nuova cartella *stili*.

![Denominazione nella nuova cartella 'Stili'](layouts/_static/image4.png)

All'interno di nuove *stili* cartella, creare un file denominato *Movies.css*.

![Creazione di un nuovo file Movies.css](layouts/_static/image5.png)

Sostituire il contenuto della nuova *CSS* file con il codice seguente:

[!code-css[Main](layouts/samples/sample2.css)]

Non parliamo di gran parte di queste regole CSS, tranne a considerare due aspetti. Una è che oltre a impostare i tipi di carattere e dimensioni, le regole di usano il posizionamento assoluto per stabilire il percorso di area del contenuto principale, intestazione, piè di pagina. Se si ha familiarità con il posizionamento in CSS, è possibile leggere il [posizionamento CSS](http://www.w3schools.com/css/css_positioning.asp) esercitazioni nel sito W3Schools.

L'altra cosa da notare è che nella parte inferiore, è stato copiato le regole di stile che facevano originariamente definito singolarmente tramite il *Movies.cshtml* file. Queste regole sono state usate nel [Introduzione alla visualizzazione dei dati da utilizzando ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) esercitazione per rendere il `WebGrid` helper il rendering del markup che l'aggiunta di strisce alla tabella. (Se si intende usare un *CSS* file per le definizioni di stile, è possibile inserire anche le regole di stile per l'intero sito in essa.)

## <a name="updating-the-movies-file-to-use-the-layout"></a>L'aggiornamento del File di film per l'uso del Layout

A questo punto è possibile aggiornare i file esistenti nel sito per usare il nuovo layout. Aprire il *Movies.cshtml* file. Nella parte superiore, come la prima riga del codice, aggiungere quanto segue:

[!code-csharp[Main](layouts/samples/sample3.cs)]

In questo modo inizia ora la pagina:

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

Questa riga di codice indica ad ASP.NET che, quando la *film* pagina è in esecuzione, deve essere unito con la  *\_layout. cshtml* file.

Poiché il *Movies.cshtml* file Usa ora una pagina di layout, è possibile rimuovere il markup dalle *Movies.cshtml* pagina che ha preso in considerazione dal  *\_layout. cshtml*file. Estrarre il `<!DOCTYPE>`, `<html>`, e `<body>` di apertura e chiusura di tag. Interessi l'intera `<head>` elemento e il relativo contenuto, che include le regole di stile per la griglia, poiché tali regole è ora disponibile in un *CSS* file. Anche se siete, modificare l'oggetto esistente `<h1>` elemento da un' `<h2>` elemento; è necessario un `<h1>` elemento nella pagina di layout già. Modifica il `<h2>` testo per "Elenco di film".

È normalmente non sarebbe necessario rendere questi tipi di modifiche in una pagina di contenuto. Quando il sito iniziare con una pagina di layout, si creano le pagine di contenuto senza tutti questi elementi per iniziare. In questo caso, tuttavia, si sta convertendo una pagina autonoma in uno che utilizza un layout, pertanto non c'è un po' di pulizia.

Quando hai finito, il *Movies.cshtml* pagina avrà un aspetto simile al seguente:

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a>Il Layout di test

A questo punto è possibile visualizzare il layout di un aspetto analogo. In WebMatrix, fare doppio clic il *Movies.cshtml* pagina e selezionare **Avvia nel browser**. Il browser visualizza la pagina, si differenzia da questa pagina:

![Pagina Movies sottoposta a rendering utilizzando un layout](layouts/_static/image6.png)

ASP.NET ha unito il contenuto della pagina Movies.cshtml nel  *\_layout. cshtml* pagina a destra dove il `RenderBody` metodo è. E ovviamente il  *\_layout. cshtml* pagina riferimenti a un *CSS* file che definisce l'aspetto della pagina.

## <a name="updating-the-addmovie-page-to-use-the-layout"></a>Aggiornamento della pagina AddMovie per l'uso del Layout

Il reale vantaggio di layout è che è possibile usarli per tutte le pagine nel sito. Aprire il *AddMovie.cshtml* pagina.

Si ricorderà che il *AddMovie.cshtml* pagina originariamente rappresentava alcune regole CSS in esso per definire l'aspetto dei messaggi di errore di convalida. Poiché è necessario un *CSS* file per ora il sito, è possibile spostare tali regole per il *CSS* file. Li rimuove dal *AddMovie.cshtml* del file e aggiungerli alla fine della *Movies.css* file. Si siano spostando le regole seguenti:

[!code-css[Main](layouts/samples/sample6.css)]

Ora apportare la stessa natura delle modifiche in *AddMovie.cshtml* che è stato fatto per *Movies.cshtml* , ovvero aggiungere `Layout="~/_Layout.cshtml;` e rimuovere il markup HTML che si trova ora estraneo. Modificare l'elemento `<h1>` in `<h2>`. Al termine, la pagina avrà un aspetto simile all'esempio:

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

Eseguire la pagina. A questo punto sembra questa illustrazione:

!['Aggiungi film' pagina sottoposta a rendering utilizzando un layout](layouts/_static/image7.png)

Si desidera apportare modifiche analoghe alle pagine del sito, ovvero *EditMovie.cshtml* e *DeleteMovie.cshtml*. Tuttavia, prima di procedere, è possibile apportare un'altra modifica al layout che rende un po' più flessibile.

## <a name="passing-title-information-to-the-layout-page"></a>Informazioni sui titoli di passare alla pagina di Layout

Il  *\_layout. cshtml* dispone di pagina in cui è stato creato un `<title>` elemento che è impostato su "Movie area sito personale". La maggior parte dei browser di visualizzare il contenuto di questo elemento come testo in una scheda:

![La pagina &lt;titolo&gt; elemento visualizzato in una scheda del browser](layouts/_static/image8.png)

Queste informazioni di titolo sono generiche. Si supponga che il testo del titolo per essere più specifici per la pagina corrente. (Il testo del titolo viene usato anche dai motori di ricerca per stabilire quale sia la pagina sulle.) È possibile passare informazioni da una pagina contenuto, ad esempio *Movies.cshtml* oppure *AddMovie.cshtml* alla pagina di layout e quindi usare tali informazioni per personalizzare quali la pagina di layout esegue il rendering.

Aprire il *Movies.cshtml* pagina nuovamente. Nel codice nella parte superiore, aggiungere la riga seguente:

[!code-csharp[Main](layouts/samples/sample8.cs)]

Il `Page` l'oggetto è disponibile in tutti i *cshtml* pagine ed è per questo scopo, vale a dire per condividere informazioni tra una pagina e il relativo layout.

Aprire il<em>\_layout. cshtml</em> pagina. Modifica il `<title>` elemento in modo che l'it è simile a questo markup:

[!code-html[Main](layouts/samples/sample9.html)]

Questo codice esegue il rendering di qualsiasi oggetto presente nel `Page.Title` proprietà a destra nel percorso specificato nella pagina.

Eseguire la *Movies.cshtml* pagina. Questa volta la scheda del browser mostra ciò che è stato passato come valore di `Page.Title`:

![Una scheda del browser che mostra il titolo creato in modo dinamico](layouts/_static/image9.png)

Se si desidera, consente di visualizzare il codice sorgente della pagina nel browser. È possibile notare che il `<title>` elemento viene sottoposto a rendering come `<title>List Movies</title>`.

> [!TIP] 
> 
> **Oggetto Page**
> 
> Una funzionalità utile dei `Page` è costituito da un oggetto dinamico, ovvero il `Title` proprietà non è un nome fisso o riservato. È possibile usare *eventuali* nome per il valore di `Page` oggetto. Ad esempio, si può facilmente aver passato il titolo usando una proprietà denominata `Page.CurrentName` o `Page.MyPage`. L'unica restrizione è che il nome deve seguire le regole normali per le proprietà che possono essere denominate. (Ad esempio, il nome non può contenere uno spazio).
> 
> È possibile passare qualsiasi numero di valori usando il `Page` oggetto. Se si desidera passare informazioni relative al filmato alla pagina di layout, è possibile passare i valori usando ad esempio `Page.MovieTitle` e `Page.Genre` e `Page.MovieYear`. In alternativa, tutti gli altri nomi che è ideato per archiviare le informazioni. L'unico requisito, ovvero che è probabilmente ovvio, è che è necessario usare gli stessi nomi nella pagina di contenuto e nella pagina di layout.
> 
> Le informazioni passate tramite il `Page` oggetto non è limitato al solo con il testo da visualizzare nella pagina di layout. È possibile passare un valore alla pagina di layout, e quindi codice nella pagina di layout può usare il valore per decidere se visualizzare una sezione della pagina, quali *CSS* per l'uso di file e così via. I valori passati nella `Page` oggetto sono, ad esempio eventuali altri valori che verrà usata nel codice. È sufficiente che i valori hanno origine nella pagina di contenuto e vengono passati alla pagina di layout.


Aprire il *AddMovie.cshtml* pagina e aggiungere una riga all'inizio del codice che fornisce un titolo per il *AddMovie.cshtml* pagina:

[!code-csharp[Main](layouts/samples/sample10.cs)]

Eseguire la *AddMovie.cshtml* pagina. Viene visualizzato il nuovo titolo:

![Una scheda del browser che mostra il titolo 'Aggiungi film' creato in modo dinamico](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a>Aggiornamento delle pagine rimanenti per l'uso del Layout

A questo punto sarà possibile completare le pagine rimanenti presenti nel sito in modo che utilizzino il nuovo layout. Aprire *EditMovie.cshtml* e *DeleteMovie.cshtml* a sua volta e apportare le stesse modifiche in ognuna.

Aggiungere la riga di codice che si collega alla pagina di layout:

[!code-csharp[Main](layouts/samples/sample11.cs)]

Aggiungere una riga per impostare il titolo della pagina:

[!code-csharp[Main](layouts/samples/sample12.cs)]

oppure:

[!code-csharp[Main](layouts/samples/sample13.cs)]

Rimuovere tutto il markup HTML estraneo, lasciare in pratica, solo i bit che si trovano all'interno di `<body>` elemento (più il blocco di codice nella parte superiore).

Modifica il `<h1>` elemento sia una `<h2>` elemento.

Dopo avere apportato queste modifiche, il test di ogni e assicurarsi che vengano visualizzate correttamente e che il titolo sia corretto.

## <a name="parting-thoughts-about-layout-pages"></a>PROD143 idee sulle pagine di Layout

In questa esercitazione è stata creata una  *\_layout. cshtml* pagina e usato la `RenderBody` metodo per unire il contenuto da un'altra pagina. Che è il modello di base per l'uso di layout nelle pagine Web.

Pagine di layout hanno funzionalità aggiuntive che non sono stati trattati qui. Ad esempio, è possibile annidare le pagine di layout, ovvero una pagina di layout può a sua volta fare riferimento a un altro. Layout annidati può essere utile se si lavora con le sezioni di un sito che richiedono layout diversi. È anche possibile usare metodi aggiuntivi (ad esempio, `RenderSection`) per configurare denominato sezioni nella pagina di layout.

La combinazione di pagine di layout e *CSS* file è potente. Come si vedrà nella serie di esercitazioni in avanti, in WebMatrix è possibile creare un sito basato su un *modello*, che offre un sito dotato di funzionalità precompilate in esso. I modelli usano buona pagine di layout e CSS per creare siti che aspetto accattivante e che dispongono di funzionalità come i menu. Ecco uno screenshot della pagina iniziale da un sito basato su un modello, che mostra le funzionalità che utilizzano pagine di layout e CSS:

![Layout creata dal modello di sito WebMatrix che mostra l'intestazione dell'area di navigazione, area di contenuto, sezione facoltativa e i collegamenti di accesso](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a>Elenco completo per la pagina di film (aggiornata per usare una pagina di Layout)

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a>Pagina completata con l'elenco per aggiungere pagina film (aggiornata per il Layout)

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a>Completare l'elenco di pagina per pagina film Delete (aggiornata per il Layout)

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a>Completare l'elenco di pagina per pagina film modifica (aggiornata per il Layout)

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a>In arrivo

Nella prossima esercitazione, si apprenderà come pubblicare il sito Internet in modo che tutti gli utenti possono visualizzarlo.

## <a name="additional-resources"></a>Risorse aggiuntive

- [Creazione di un aspetto coerente](https://go.microsoft.com/fwlink/?LinkID=202891) , ovvero un articolo che fornisce maggiori dettagli sull'uso dei layout. Viene inoltre descritto come passare un valore a una pagina di layout che mostra o nasconde la parte del contenuto.
- [Annidati pagine di Layout con Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) , ovvero il blog di Mike Brind un esempio di come annidare le pagine di layout. (Include il download delle pagine).

> [!div class="step-by-step"]
> [Precedente](deleting-data.md)
> [Successivo](publishing.md)
