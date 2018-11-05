---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: "Introduzione a ASP.NET Web Pages: l'eliminazione dei dati di Database | Microsoft Docs"
author: Rick-Anderson
description: Questa esercitazione illustra come eliminare una voce di singoli database. Si presuppone di che aver completato la serie tramite l'aggiornamento di dati del Database nell'indirizzo PA Web di ASP.NET....
ms.author: riande
ms.date: 01/02/2018
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: b2ef8fcc8cc534bd31fea83bf0b085b85995f417
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020442"
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a>Introduzione a pagine Web ASP.NET: l'eliminazione dei dati di Database
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questa esercitazione illustra come eliminare una voce di singoli database. Si presuppone di aver completato la serie attraverso [l'aggiornamento dei dati di un Database in ASP.NET Web Pages](updating-data.md).
> 
> Che cosa si apprenderà come:
> 
> - Come selezionare un singolo record da un elenco di record.
> - Come eliminare un record singolo da un database.
> - Come verificare che un pulsante specifico è stato fatto clic in un form.
>   
> 
> Le funzionalità/tecnologie illustrate:
> 
> - Il `WebGrid` helper.
> - Il codice SQL `Delete` comando.
> - Il `Database.Execute` metodo per l'esecuzione di un database SQL `Delete` comando.


## <a name="what-youll-build"></a>Scopo dell'esercitazione

Nell'esercitazione precedente, è stato descritto come aggiornare un record di database esistente. Questa esercitazione è simile, ad eccezione del fatto che anziché aggiornare il record, è possibile eliminarlo. I processi sono molto simile, ad eccezione del fatto che l'eliminazione è più semplice, in modo che questa esercitazione saranno a breve.

Nel *film* pagina, si aggiornerà il `WebGrid` helper, in modo che venga visualizzato un **eliminare** collegamento accanto a ogni film di accompagnamento per il **modifica** collegamento aggiunto in precedenza.

![Pagina Movies che mostra un collegamento di eliminazione per ogni film](deleting-data/_static/image1.png)

Come con la modifica, quando si sceglie la **eliminare** collegamento, consente a un'altra pagina, in cui le informazioni relative al filmato è già in un form:

![Elimina pagina film con un film visualizzato](deleting-data/_static/image2.png)

È quindi possibile fare clic sul pulsante per eliminare definitivamente il record.

## <a name="adding-a-delete-link-to-the-movie-listing"></a>Aggiunta di un collegamento di eliminazione per l'elenco di film

Si inizia aggiungendo un **eliminare** collegare le `WebGrid` helper. Questo collegamento è simile al **modifica** collegamento è stato aggiunto nell'esercitazione precedente.

Aprire il *Movies.cshtml* file.

Modifica il `WebGrid` markup nel corpo della pagina aggiungendo una colonna. Ecco il markup modificato:

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

La nuova colonna è questa:

[!code-html[Main](deleting-data/samples/sample2.html)]

Il modo in cui la griglia è configurata, il **Edit** colonna è più a sinistra nella griglia e il **eliminare** colonna è più a destra. (È presente una virgola dopo il `Year` colonna a questo punto, nel caso in cui non si noti che.) Non c'è niente di speciale in cui vengono inseriti queste colonne di collegamento ed è possibile inserire facilmente uno accanto a altro. In questo caso, risultano separate per renderli più difficile da ottenere confuse.

![Pagina di film con collegamenti di modifica e dettagli contrassegnata per mostrare che non si tratta di uno accanto a altro](deleting-data/_static/image3.png)

La nuova colonna viene visualizzato un collegamento (`<a>` elemento) con il testo "Elimina". La destinazione del collegamento (relativi `href` attributi) è il codice che in definitiva viene risolto simile a questo URL, con la `id` valore diverso per ogni film:

[!code-css[Main](deleting-data/samples/sample3.css)]

Questo collegamento verrà richiamato una pagina denominata *DeleteMovie* e passare l'ID del film selezionato.

Questa esercitazione non descritte in dettaglio come costruire il collegamento, in quanto è quasi identico al **Edit** collegamento dall'esercitazione precedente ([aggiornamento dei dati di un Database in ASP.NET Web Pages](updating-data.md)).

## <a name="creating-the-delete-page"></a>Creazione della pagina Delete

Ora è possibile creare la pagina che sarà la destinazione per il **eliminare** collegamento nella griglia.

> [!NOTE] 
> 
> **Importante** la tecnica di selezionando prima un record da eliminare e quindi usando una pagina separata e un pulsante per confermare il processo è estremamente importante per la sicurezza. Come già detto nelle esercitazioni precedenti, rendendo *eventuali* ordinamento della modifica al sito Web deve *sempre* essere eseguita tramite un modulo &mdash; , ovvero mediante un'operazione HTTP POST. Se si è stato possibile modificare il sito appena selezionando un collegamento (vale a dire usando un'operazione GET), le persone è stato possibile effettuare richieste semplici per il sito ed eliminare i dati. Anche un agente di ricerca del motore di ricerca che indicizza il sito è stato possibile eliminare inavvertitamente dati solo dai collegamenti seguenti.
> 
> Quando l'app consente agli utenti di modificare un record, è necessario presentare all'utente il record per la modifica comunque. Ma si potrebbe essere tentati di ignorare questo passaggio per l'eliminazione di un record. Non ignorare questo passaggio, tuttavia. (È anche utile per gli utenti visualizzino il record e confermare che si vuole eliminare il record che sono destinate.)
> 
> In una serie di esercitazioni successive, verrà illustrato come aggiungere funzionalità di accesso in modo che un utente deve accedere prima di eliminare un record.


Creare una pagina denominata *DeleteMovie.cshtml* e sostituire quello presente nel file con il markup seguente:

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

Questo markup è simile al *EditMovie* pagine, con la differenza che invece di usare le caselle di testo (`<input type="text">`), incluso il markup `<span>` elementi. Non è niente qui da modificare. Tutto è necessario eseguire è di visualizzare i dettagli del film in modo che gli utenti possono assicurarsi che si vuole eliminare il film a destra.

Il markup contiene già un collegamento che consente all'utente di tornare alla pagina di elenco di film.

Ad esempio la *EditMovie* pagina, l'ID del film selezionato viene archiviato in un campo nascosto. (Viene passato nella pagina in primo luogo come valore stringa di query.) È presente un `Html.ValidationSummary` chiamata che verrà visualizzati gli errori di convalida. In questo caso, l'errore potrebbe essere che nessun ID del film è stato passato alla pagina o che l'ID del film non è valido. Questa situazione può verificarsi se un utente è stata eseguita questa pagina senza selezionare prima un film nel *film* pagina.

Didascalia del pulsante viene **eliminare film**, e l'attributo name è impostato su `buttonDelete`. Il `name` attributo verrà utilizzato nel codice per identificare il pulsante di invio del modulo.

È possibile scrivere codice per 1) leggere i dettagli del film quando la pagina viene visualizzata prima di tutto e 2) effettivamente eliminare il film quando l'utente fa clic sul pulsante.

## <a name="adding-code-to-read-a-single-movie"></a>Aggiunta di codice per leggere un singolo filmato

In cima il *DeleteMovie.cshtml* pagina, aggiungere il blocco di codice seguente:

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

Questo markup è quello utilizzato per il codice corrispondente nel *EditMovie* pagina. Ottiene l'ID del film all'esterno di stringa di query e Usa l'ID per leggere un record dal database. Il codice include il test di convalida (`IsInt()` e `row != null`) per assicurarsi che l'ID del film viene passata alla pagina sia valido.

Tenere presente che questo codice deve essere eseguito solo alla prima che esecuzione della pagina. Non si vuole leggere nuovamente il record di film dal database quando l'utente sceglie il **film eliminare** pulsante. Pertanto, il codice per leggere il film si trova all'interno di un test con la dicitura `if(!IsPost)` &mdash; vale a dire *se la richiesta non è un'operazione post (invio del modulo)*.

## <a name="adding-code-to-delete-the-selected-movie"></a>Aggiunta di codice per eliminare il film selezionato

Per eliminare il film quando l'utente fa clic sul pulsante, aggiungere il codice seguente all'interno di parentesi graffa di chiusura di `@` blocco:

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

Questo codice è simile al codice per l'aggiornamento di un record esistente, ma più semplice. Il codice viene eseguito essenzialmente un SQL `Delete` istruzione.

 Come nel *EditMovie* pagina, il codice si trova in un `if(IsPost)` blocco. Questa volta il `if()` condizione è un po' più complicata: 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

Esistono due condizioni qui. Il primo è che viene inviata la pagina, come si è visto prima &mdash; `if(IsPost)`.

La seconda condizione è `!Request["buttonDelete"].IsEmpty()`, vale a dire che la richiesta include un oggetto denominato `buttonDelete`. È vero, è un metodo indiretto d' quale pulsante di invio del modulo di test. Se un modulo contiene più pulsanti di invio, viene visualizzato solo il nome del pulsante su cui è stato fatto clic nella richiesta. Pertanto, in modo logico, se il nome di un particolare pulsante viene visualizzato nella richiesta &mdash; o come indicato nel codice, se tale pulsante non è vuoto &mdash; che corrisponde al pulsante che ha inviato il form.

Il `&&` operatore significa "e" (AND logico). Di conseguenza l'intera `if` condizione è...

*Questa richiesta è una richiesta post (non una richiesta di primo)*  
  
 AND  
  
*Il* `buttonDelete` *pulsante si tratta del pulsante che ha inviato il form.*

Questo modulo (in effetti, questa pagina) contiene un solo pulsante, in modo che il test aggiuntivo per `buttonDelete` tecnicamente non è obbligatorio. Tuttavia, si sta per eseguire un'operazione che verrà rimossi in modo permanente i dati. Pertanto, si desidera come assicurarsi che si sta eseguendo l'operazione solo quando l'utente ha richiesto in modo esplicito viene possibile. Si supponga, ad esempio, è espanso in un secondo momento in questa pagina e aggiungervi altri pulsanti. Anche in questo caso il codice che elimina il film verrà eseguito solo se il `buttonDelete` è stato fatto clic sul pulsante.

Ad esempio la *EditMovie* pagina, ottenere l'ID del campo nascosto e quindi eseguire il comando SQL. La sintassi per il `Delete` istruzione è:

`DELETE FROM table WHERE ID = value`

È fondamentale per includere il `WHERE` clausola e l'ID. Se si omette la clausola WHERE *verranno eliminati tutti i record nella tabella*. Come si è visto, il valore ID è passare al comando SQL utilizzando un segnaposto.

## <a name="testing-the-movie-delete-process"></a>Testare il processo di eliminazione di film

A questo punto è possibile testare. Eseguire la *film* e fare clic su **eliminare** accanto a un filmato. Quando la *DeleteMovie* verrà visualizzata la pagina, fare clic su **eliminare film**.

![Elimina pagina film con il pulsante Elimina film evidenziato](deleting-data/_static/image4.png)

Quando si fa clic sul pulsante, il codice elimina il film e restituisce l'elenco di film. Non esiste è possibile cercare il film eliminato e verificare che è stata eliminata.

## <a name="coming-up-next"></a>In arrivo

L'esercitazione successiva illustra come concedere a tutte le pagine nel sito di un aspetto comune e il layout.

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a>Elenco completo per la pagina di film (aggiornata con collegamenti di eliminazione)

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a>Elenco completo per la pagina DeleteMovie

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a>Risorse aggiuntive

- [Introduzione alla programmazione Web ASP.NET usando la sintassi Razor](../introducing-razor-syntax-c.md)
- [Istruzione SQL DELETE](http://www.w3schools.com/sql/sql_delete.asp) sul sito W3Schools

> [!div class="step-by-step"]
> [Precedente](updating-data.md)
> [Successivo](layouts.md)
