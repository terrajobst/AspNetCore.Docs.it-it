---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: Aggiunta di una nuova categoria a DropDownList tramite jQuery UI | Documenti Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 16f7af1d679aace24fff86abb19740beebafe785
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871935"
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a>Aggiunta di una nuova categoria a DropDownList tramite jQuery UI
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

Il codice HTML `Select` tag è ideale per presentare un elenco di dati di categoria predefinito, ma spesso è necessario aggiungere una nuova categoria. Si supponga che si vuole aggiungere il genere "Opera" per le categorie dal database? In questa sezione, si utilizzerà jQuery UI per aggiungere una finestra di dialogo che è possibile utilizzare per aggiungere una nuova categoria. L'immagine seguente mostra come l'interfaccia utente sarà presente nel browser.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

Quando un utente seleziona il **aggiungere nuovo Genre** collegamento, una finestra di dialogo popup richiede l'immissione di un nuovo nome di genere (e facoltativamente una descrizione). L'immagine seguente mostra il **aggiungere Genre** finestra di dialogo popup.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

Quando viene immesso un nuovo nome genre e **salvare** pulsante è premuto, si verifica quanto segue:

1. Una chiamata AJAX invia i dati per il metodo di creazione del Controller Genre, che salva il genere di nuovo nel database e restituisce le nuove informazioni di genere (nome genre e ID) in formato JSON.
2. JavaScript aggiunge i nuovi dati genre all'elenco di selezione.
3. JavaScript rende il genere di nuovo l'elemento selezionato.

   Nell'immagine seguente, **Opera** è stato aggiunto al database e selezionato il **Genre** nell'elenco a discesa. 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

Aprire il *Views\StoreManager\Create.cshtml* file e sostituire il markup genre con quanto segue nel codice seguente:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

Il `_ChooseGenre` contiene tutta la logica per associare le JavaScript e jQuery utilizzata per implementare la funzionalità di genere Aggiungi nuova visualizzazione parziale. È stato completato il codice sarà semplice eseguire la stessa operazione con l'artista dell'interfaccia utente.

In Esplora soluzioni, fare clic destro la *Views\StoreManager* cartella e selezionare **Aggiungi**, quindi **vista**. Nel **nome vista** di input, immettere `_ChooseGenre` selezionare **Aggiungi**. Sostituire il markup di *Views\StoreManager\\_ChooseGenre.cshtml* file con le operazioni seguenti:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

La prima riga dichiara che si sta passando un `Album` come modello in esame esattamente lo stesso modello istruzione disponibili nella visualizzazione di creazione. Le righe successive sono il **etichetta** markup di supporto. La riga successiva è il **DropDownList** helper chiama, esattamente come la visualizzazione di creazione originale. La riga successiva viene aggiunto un collegamento con il nome `Add New Genre`, e stili ad esempio un pulsante. La riga contenente `ValidationMessageFor` viene copiato direttamente dalla visualizzazione di creazione. Le righe seguenti:

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

Crea un elemento div nascosto, con l'ID di `genreDialog`. Utilizzeremo jQuery per collegare il nostro **aggiungere Genre** la finestra di dialogo con l'ID `genreDialog` in questo tag div. Le ultime due tag script contengono collegamenti ai file JavaScript che verrà utilizzato per implementare la funzionalità in genere nuovo Aggiungi. Il */Scripts/chooseGenre.js* file viene fornito automaticamente nel progetto, verrà esaminato, più avanti nell'esercitazione.

Eseguire l'applicazione e fare clic su di **aggiungere nuovo Genre** pulsante. Nel **aggiungere Genre** finestra di dialogo immettere **Opera** nel **nome** casella di input.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

Fare clic sul pulsante **Salva**. Una chiamata AJAX crea la categoria Opera e quindi popola l'elenco a discesa con Opera e imposta Opera come il genere selezionato.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

Immettere un artista, titolo e prezzo, quindi selezionare il **crea** pulsante. Se si immette un prezzo inferiore a $8.99, il nuovo album verrà visualizzato nella parte superiore della visualizzazione dell'indice. Verificare che la nuova voce album è stata salvata nel database.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

Provare a creare un nuovo genere con solo una lettera. Nell'esempio di codice il *Models\Genre.cs* file imposta la lunghezza minima e massima del nome del genere.

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

La convalida lato client segnala che è necessario immettere una stringa di caratteri compreso tra 2 e 20.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a>Esaminare come un genere nuovo viene aggiunto al Database e l'elenco Select.

Aprire il *Scripts\chooseGenre.js* file ed esaminare il codice.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

La seconda riga viene utilizzato l'ID `genreDialog` per creare una finestra di dialogo nel tag div nel *Views\StoreManager\\_ChooseGenre.cshtml* file. La maggior parte dei parametri denominati sono necessita di spiegazione. Il `autoOpen` parametro è impostato su false, selezionando il **creare Genre** pulsante verrà aperta la finestra di dialogo in modo esplicito (descritto quest'ultimo su). La finestra di dialogo include due pulsanti, **salvare** e **Annulla**. Il **Annulla** pulsante consente di chiudere la finestra di dialogo. Il codice seguente illustra il **salvare** pulsante funzione.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

Il `var createGenreForm` sia selezionato il `createGenreForm` ID. Il `createGenreForm` ID è stato impostato nel codice seguente, vedere il *Views\Genre\\_CreateGenre.cshtml* file.

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

Il [Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) overload helper utilizzato nel *Views\Genre\\_CreateGenre.cshtml* file genera HTML con un attributo action contenente l'URL per inviare il form. È possibile visualizzare questa visualizzazione della pagina di album create in un browser e selezionando Mostra origine nel browser. Di seguito viene illustrato il codice HTML generato contenente il tag form.

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

Di jQuery `$.post` riga esegue una chiamata di AJAX per l'attributo action (`/StoreManager/Create`) e passa i dati di **Genre creare** la finestra di dialogo. I dati sono costituiti da nome per il nuovo genre e una descrizione facoltativa. Se la chiamata di AJAX ha esito positivo, il nuovo nome genre e il valore vengono aggiunti al markup selezionare e il nuovo genere è impostato sul valore selezionato. Poiché si tratta di markup generato dinamicamente, è possibile visualizzare la nuova opzione di seleziona la visualizzazione di origine nel browser. È possibile visualizzare la nuova pagina HTML con gli strumenti di sviluppo F12 di Internet Explorer 9. Per visualizzare la nuova opzione di seleziona, in Internet Explorer 9, premere il tasto F12 per avviare gli strumenti di sviluppo F12. Passare alla pagina di creare e aggiungere un nuovo genere in modo che il genere nuovo è selezionato nell'elenco di selezione genere. In strumenti di sviluppo F12:

1. Selezionare la scheda HTML.
2. Raggiunto l'icona di aggiornamento.  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. Nella casella di ricerca, immettere GenreID.
4. Utilizzando l'icona,   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   individuare il tag di selezione seguente:

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. Espandere l'ultimo valore di opzione.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

Nell'esempio di codice il *Scripts\chooseGenre.js* file illustra il **aggiungere nuovo Genre** pulsante ottiene collegato all'evento, fare clic su e il modo in **aggiungere nuovo Genre** è la finestra di dialogo creato.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

La prima riga crea collegata a una funzione di fare clic su di **aggiungere nuovo Genre** pulsante. Il markup seguente dal Views\StoreManager\\_ChooseGenre.cshtml file Mostra come **aggiungere nuovo Genre** pulsante è stato creato:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

Il metodo load crea e apre la finestra di dialogo Aggiungi Genre e chiama il jQuery `parse` metodo pertanto la convalida del client si verifica nei dati immessi nella finestra di dialogo.

In questa sezione si è appreso come creare una finestra di dialogo che consente di aggiungere nuovi dati di categoria a un elenco di selezione. È possibile seguire la stessa procedura per creare l'interfaccia utente per aggiungere un nuovo artista all'elenco di selezione artista. In questa esercitazione è assegnato a una panoramica dell'utilizzo con l'helper HTML di ASP.NET MVC **DropDownList**. Per ulteriori informazioni sull'utilizzo di **DropDownList**, vedere la sezione riferimenti aggiunta. Saremmo lieti di sapere se questa esercitazione è stato utile.

Rick.Anderson[at]Microsoft.com

### <a name="additional-references"></a>Riferimenti aggiuntivi

- [ASP.NET MVC – CSS gli elenchi a discesa esercitazione](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) da [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- [Scelto](http://harvesthq.github.com/chosen/) plug-in A JavaScript che supportano la selezione multipla e filtro.

### <a name="contributors"></a>Contributors

- [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- Jean-Sébastien Goupil
- [Blog di Brad Wilson](http://bradwilson.typepad.com/)

### <a name="reviewers"></a>Revisori

- Jean-Sébastien Goupil
- [Blog di Brad Wilson](http://bradwilson.typepad.com/)
- Mike Pope
- Tom Dykstra

> [!div class="step-by-step"]
> [Precedente](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
