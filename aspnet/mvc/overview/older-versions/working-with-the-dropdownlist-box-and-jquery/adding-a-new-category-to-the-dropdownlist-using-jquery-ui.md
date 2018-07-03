---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: Aggiunta di una nuova categoria a DropDownList tramite l'interfaccia utente di jQuery | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 5d23b1d41834d669bb8006ae0ac963fab60918dc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402993"
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a>Aggiunta di una nuova categoria a DropDownList tramite l'interfaccia utente di jQuery
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

Il codice HTML `Select` tag è ideale per presentare un elenco di dati della categoria fisso, ma spesso è necessario aggiungere una nuova categoria. Si supponga che si vuole aggiungere il genere "Opera" per le categorie nel nostro database? In questa sezione, useremo l'indirizzo dell'interfaccia utente di jQuery per aggiungere una finestra di dialogo che è possibile usare per aggiungere una nuova categoria. L'immagine seguente mostra come l'interfaccia utente verrà visualizzate nel browser.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

Quando un utente seleziona il **Aggiungi nuovo Genre** collegamento, una finestra di dialogo popup richiede l'immissione di un nuovo nome di genere (e facoltativamente una descrizione). L'immagine seguente mostra le **Genre aggiungere** finestra di dialogo popup.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

Quando viene immesso un nuovo nome al genere e il **salvare** pulsante è premuto, si verifica quanto segue:

1. Una chiamata AJAX invia i dati per il metodo di creazione del Controller Genre, che salva il genere di nuovo il database e restituisce le nuove informazioni di genere (genere nome e ID) in formato JSON.
2. JavaScript aggiunge i nuovi dati genre all'elenco di selezione.
3. JavaScript rende il genere di nuovo l'elemento selezionato.

   Nell'immagine seguente, **Opera** è stato aggiunto al database e selezionato nella **Genre** nell'elenco a discesa. 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

Aprire il *Views\StoreManager\Create.cshtml* file e sostituire il markup del genere con il codice seguente il codice seguente:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

Il `_ChooseGenre` contiene tutta la logica per associare il JavaScript e jQuery utilizzata per implementare la funzionalità di genere Aggiungi nuova visualizzazione parziale. Dopo che è stata completata il codice sarà semplice eseguire la stessa operazione con l'interfaccia utente di artista.

In Esplora soluzioni fare clic con il pulsante destro il *Views\StoreManager* cartella e selezionare **Add**, quindi **visualizzazione**. Nel **nome della vista** di input, immettere `_ChooseGenre` quindi selezionare **Add**. Sostituire il markup nel *Views\StoreManager\\_ChooseGenre.cshtml* file con il codice seguente:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

La prima riga dichiara che si sta passando un `Album` come il nostro modello, esattamente allo stesso modo del modello istruzione trovata nella visualizzazione di creazione. Le righe successive sono le **etichetta** markup dell'helper. La riga successiva è la **DropDownList** helper chiama, esattamente come in visualizzazione di creazione originale. La riga successiva viene aggiunto un collegamento con il nome `Add New Genre`, e consente di disegnare come un pulsante. La riga contenente `ValidationMessageFor` viene copiato direttamente dalla visualizzazione di creazione. Le righe seguenti:

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

Crea un elemento div nascosto, con l'ID di `genreDialog`. Si userà jQuery collegherà nostro **Genre aggiungere** finestra di dialogo con l'ID `genreDialog` in questo elemento DIV. Le ultime due tag di script contengono collegamenti per i file JavaScript che verrà usato per implementare la funzionalità genre nuovo componente. Il */Scripts/chooseGenre.js* file viene fornito per l'utente nel progetto, verranno esaminate, più avanti nell'esercitazione.

Eseguire l'applicazione e fare clic sui **Aggiungi nuovo Genre** pulsante. Nel **aggiungere Genre** finestra di dialogo immettere **Opera** nel **nome** casella di input.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

Fare clic sul pulsante **Salva**. Una chiamata AJAX consente di creare la categoria Opera e quindi popola l'elenco a discesa con Opera e imposta Opera come il genere selezionato.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

Immettere un artista, title e prezzo e quindi selezionare il **Create** pulsante. Se si immette un prezzo minore di $8.99, il nuovo album verranno visualizzati nella parte superiore della visualizzazione Index. Verificare che la nuova voce di album è stata salvata nel database.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

Provare a creare un nuovo genre con solo una lettera. Il codice seguente il *Models\Genre.cs* file imposta la lunghezza minima e massima del nome del genere.

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

La convalida lato client segnala che è necessario immettere una stringa compresa tra 2 e 20 caratteri.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a>Esaminando come un genere nuova viene aggiunto al Database e l'elenco Select.

Aprire il *Scripts\chooseGenre.js* file ed esaminare il codice.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

La seconda riga Usa l'ID `genreDialog` per creare una finestra di dialogo nel tag div nel *Views\StoreManager\\_ChooseGenre.cshtml* file. La maggior parte dei parametri denominati sono necessita di spiegazione. Il `autoOpen` parametro è impostato su false, selezionando il **Genre creare** pulsante verrà aperta la finestra di dialogo in modo esplicito (illustrato quest'ultimo su). La finestra di dialogo ha due pulsanti, **salvare** e **Annulla**. Il **annullare** pulsante chiude la finestra di dialogo. Il codice seguente illustra il **salvare** pulsante (funzione).

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

Il `var createGenreForm` viene selezionato il `createGenreForm` ID. Il `createGenreForm` ID è stato impostato nel seguente codice trovato nel *Views\Genre\\_CreateGenre.cshtml* file.

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

Il [Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) overload helper utilizzato nel *Views\Genre\\_CreateGenre.cshtml* file genera codice HTML con un attributo di azione che contiene l'URL per inviare il modulo. È possibile verificarlo visualizzando la pagina di album create in un browser e selezionando origine Mostra nel Visualizzatore. Il markup seguente mostra il codice HTML generato che contiene il tag form.

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

Il componente aggiuntivo jQuery `$.post` riga effettua una chiamata AJAX all'attributo dell'azione (`/StoreManager/Create`) e passa i dati dalle **Genre creare** nella finestra di dialogo. I dati sono costituiti da nome per il genere di nuovo e una descrizione facoltativa. Se la chiamata AJAX ha esito positivo, il nuovo nome al genere e il valore vengono aggiunti al markup Select e il genere nuova è impostato sul valore selezionato. Poiché si tratta di markup generata in modo dinamico, è possibile visualizzare la nuova opzione Seleziona per visualizzare il codice sorgente nel browser. È possibile visualizzare il nuova pagina HTML con strumenti di sviluppo F12 di Internet Explorer 9. Per visualizzare la nuova opzione select, in Internet Explorer 9, premere il tasto F12 per avviare gli strumenti di sviluppo F12. Passare alla pagina Create e aggiungere un nuovo genre in modo che il genere nuovo è selezionato nell'elenco di selezione genre. In strumenti di sviluppo F12:

1. Selezionare la scheda HTML.
2. Premere l'icona di aggiornamento.  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. Nella casella di ricerca immettere GenreID.
4. Usando l'icona Avanti,   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   individuare il tag di selezione seguente:

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. Espandere l'ultimo valore di opzione.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

Il seguente codice nel *Scripts\chooseGenre.js* file illustra la procedura il **aggiungere nuovo Genre** pulsante ottiene connesso per l'evento click e il comportamento della **aggiungere nuovo Genre** è la finestra di dialogo creato.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

La prima riga crea una funzione di fare clic su collegato per il **aggiungere nuovo Genre** pulsante. Il markup seguente dal Views\StoreManager\\_ChooseGenre.cshtml file Mostra come il **Aggiungi nuovo Genre** pulsante viene creato:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

Il metodo load crea e apre la finestra di dialogo Aggiungi Genre e chiama il componente aggiuntivo jQuery `parse` metodo in modo che la convalida del client si verifica nei dati immessi nella finestra di dialogo.

In questa sezione si è appreso come creare una finestra di dialogo che consente di aggiungere nuovi dati di categoria a un elenco di selezione. È possibile seguire la stessa procedura per creare l'interfaccia utente per aggiungere un nuovo artista all'elenco di selezione artista. Questa esercitazione ha presentato una panoramica dell'utilizzo con l'helper HTML di ASP.NET MVC **DropDownList**. Per altre informazioni sull'uso con il **DropDownList**, vedere la sezione riferimenti di addizione riportato di seguito. È possibile comunicarlo se in questa esercitazione è stato utile.

Rick.Anderson[at]Microsoft.com

### <a name="additional-references"></a>Riferimenti aggiuntivi

- [ASP.NET MVC: Cascading gli elenchi a discesa esercitazione](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) da [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- [Scelta](http://harvesthq.github.com/chosen/) plug-in A JavaScript che supportano la selezione multipla e filtro.

### <a name="contributors"></a>Contributors

- [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- Jean-Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)

### <a name="reviewers"></a>Revisori

- Jean-Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)
- Mike Pope
- Tom Dykstra

> [!div class="step-by-step"]
> [Precedente](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
