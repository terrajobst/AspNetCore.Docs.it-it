---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: 'Introduzione a ASP.NET Web Pages: eliminazione di dati di Database | Documenti Microsoft'
author: tfitzmac
description: In questa esercitazione viene illustrato come eliminare una voce di singoli database. Si presuppone di che aver completato la serie tramite l'aggiornamento di dati di Database in PA Web di ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: 146199e862cd6fa2607671d31633476b1cb67021
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a>Introduzione a ASP.NET Web Pages: eliminazione di dati di Database
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questa esercitazione viene illustrato come eliminare una voce di singoli database. Si presuppone di aver completato la serie tramite [aggiornamento dei dati del Database in ASP.NET Web Pages](updating-data.md).
> 
> Illustra quanto segue:
> 
> - Come selezionare un singolo record da un elenco di record.
> - Come eliminare un singolo record da un database.
> - Come verificare che un pulsante specifico è stato fatto clic in un form.
>   
> 
> Funzionalità/tecnologie descritte:
> 
> - Il `WebGrid` helper.
> - L'istruzione SQL `Delete` comando.
> - Il `Database.Execute` metodo per l'esecuzione di un database SQL `Delete` comando.


## <a name="what-youll-build"></a>Cosa Compilerai

Nell'esercitazione precedente, è stato descritto come aggiornare un record di database esistente. In questa esercitazione è simile, ad eccezione del fatto che invece di aggiornare il record, verranno eliminati. I processi sono in modo analogo, ad eccezione del fatto che l'eliminazione è più semplice, in modo che sia breve in questa esercitazione.

Nel *filmati* pagina verrà aggiornata la `WebGrid` helper in modo che venga visualizzato un **eliminare** collegamento accanto a ogni film di accompagnamento per il **modifica** collegamento aggiunto in precedenza.

![Pagina di filmati che mostra un collegamento di eliminazione per ogni film](deleting-data/_static/image1.png)

Come con la modifica, quando si sceglie il **eliminare** collegamento, consente di visualizzare una pagina diversa, in cui le informazioni di film sono già in un formato:

![Elimina pagina film con un filmato visualizzato](deleting-data/_static/image2.png)

È quindi possibile fare clic sul pulsante per eliminare definitivamente il record.

## <a name="adding-a-delete-link-to-the-movie-listing"></a>Aggiunta di un collegamento di eliminazione per l'elenco di film

Iniziare aggiungendo un **eliminare** collegare il `WebGrid` helper. Questo collegamento è simile al **modifica** collegamento è stato aggiunto in un'esercitazione precedente.

Aprire il *Movies.cshtml* file.

Modifica il `WebGrid` markup nel corpo della pagina aggiungendo una colonna. Di seguito è riportato il codice modificato:

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

Questa è la nuova colonna:

[!code-html[Main](deleting-data/samples/sample2.html)]

La configurazione della griglia, il **modifica** colonna è più a sinistra nella griglia e **eliminare** colonna è più a destra. (È presente una virgola dopo il `Year` colonna ora, nel caso in cui non si noti che.) Non c'è niente di speciale su esiti di queste colonne di collegamento e, è possibile inserire facilmente uno accanto a altro. In questo caso, ma sono separati in modo da renderle più difficile ottenere confuse.

![Pagina di filmati con collegamenti di modifica e dettagli contrassegnata per mostrare che non sono uno accanto a altro](deleting-data/_static/image3.png)

La nuova colonna viene illustrato un collegamento (`<a>` elemento) con il testo "Delete". La destinazione del collegamento (relativo `href` attributo) è il codice che alla fine si risolve in un risultato simile a questo URL, con la `id` valore diverso per ogni film:

[!code-css[Main](deleting-data/samples/sample3.css)]

Questo collegamento verrà richiamato una pagina denominata *DeleteMovie* e passare l'ID del filmato selezionata.

In questa esercitazione non descritte in dettaglio come questo collegamento viene costruito, perché è quasi identica a quella di **modifica** collegamento dall'esercitazione precedente ([aggiornamento dei dati del Database in ASP.NET Web Pages](updating-data.md)).

## <a name="creating-the-delete-page"></a>Creazione pagina di eliminazione

Ora è possibile creare la pagina che sarà la destinazione per il **eliminare** collegamento nella griglia.

> [!NOTE] 
> 
> **Importante** la tecnica di selezionando prima un record da eliminare e quindi utilizzando una pagina separata e un pulsante per verificare il processo è estremamente importante per la sicurezza. Come è stato letto esercitazioni precedenti, rendendo *qualsiasi* tipo di modifica per il sito Web deve *sempre* essere eseguita tramite un modulo &mdash; , ovvero tramite un'operazione HTTP POST. Se si è stato possibile modificare il sito semplicemente facendo clic su un collegamento (con un'operazione GET), utenti potrebbero apportare semplici richieste al sito ed eliminare i dati. Anche un crawler di motore di ricerca che l'indicizzazione del sito è stato possibile eliminare accidentalmente dati solo per i collegamenti seguenti.
> 
> Quando l'app consente a persone di modificare un record, è necessario presentare all'utente per la modifica comunque il record. Ma potrebbe essere tentati di ignorare questo passaggio per l'eliminazione di un record. Non ignorare questo passaggio, tuttavia. (È anche utile per gli utenti di visualizzare il record e confermare che si vuole eliminare il record che devono.)
> 
> In un set di esercitazioni successive, si noterà come aggiungere funzionalità di accesso in modo che un utente deve accedere prima di eliminare un record.


Creare una pagina denominata *DeleteMovie.cshtml* e sostituire quello presente nel file con il markup seguente:

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

Questo tag è simile al *EditMovie* pagine, con la differenza che anziché utilizzare le caselle di testo (`<input type="text">`), incluso il markup `<span>` elementi. Non è seguito da modificare. Sarà sufficiente è di visualizzare i dettagli del film in modo che gli utenti possono assicurarsi elimini i film destra.

Il markup contiene già un collegamento che consente all'utente di tornare alla pagina di elenco di film.

Ad esempio il *EditMovie* pagina, l'ID del filmato selezionato viene archiviato in un campo nascosto. (Viene passato alla pagina in primo luogo come un valore di stringa di query.) È presente un `Html.ValidationSummary` chiamata che consente di visualizzare gli errori di convalida. In questo caso, l'errore potrebbe essere che nessun ID film è stato passato alla pagina o che l'ID del film non è valido. Questa situazione può verificarsi se un utente è stato eseguito in questa pagina senza selezionare un filmato nel *filmati* pagina.

Didascalia del pulsante è **eliminare film**, e il relativo nome di attributo è impostato su `buttonDelete`. Il `name` verrà utilizzato l'attributo nel codice per identificare il pulsante di invio del modulo.

È necessario scrivere codice per 1) leggere i dettagli del film quando la pagina viene visualizzata prima e 2) le elimina il filmato quando l'utente fa clic sul pulsante.

## <a name="adding-code-to-read-a-single-movie"></a>Aggiungere il codice per leggere un singolo filmato

Nella parte superiore del *DeleteMovie.cshtml* pagina, aggiungere il blocco di codice seguente:

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

In questo markup corrisponde al codice corrispondente nella *EditMovie* pagina. Ottiene l'ID film fuori la stringa di query e Usa l'ID per leggere un record dal database. Il codice include il test di convalida (`IsInt()` e `row != null`) per assicurarsi che l'ID di film passato alla pagina è valido.

Tenere presente che questo codice deve essere eseguito solo alla prima che esecuzione della pagina. Non si desidera leggere nuovamente il record di film dal database quando l'utente sceglie il **eliminare film** pulsante. Pertanto, il codice per leggere il film si trova all'interno di un test che indica che `if(!IsPost)` &mdash; , ovvero *se la richiesta non è un'operazione post (invio)*.

## <a name="adding-code-to-delete-the-selected-movie"></a>Aggiungere il codice per eliminare il filmato selezionato

Per eliminare il filmato quando l'utente fa clic sul pulsante, aggiungere il seguente codice all'interno di parentesi graffa di chiusura di `@` blocco:

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

Questo codice è simile al codice per l'aggiornamento di un record esistente, ma è più semplice. In pratica viene eseguito il codice SQL `Delete` istruzione.

 Ad esempio il *EditMovie* pagina, il codice è in un `if(IsPost)` blocco. Questa volta il `if()` condizione è leggermente più complessa: 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

Esistono due condizioni. Il primo è che la pagina viene inviata, come si è visto prima &mdash; `if(IsPost)`.

È la seconda condizione `!Request["buttonDelete"].IsEmpty()`, vale a dire che la richiesta include un oggetto denominato `buttonDelete`. È vero, è un modo indiretto del test il pulsante di invio del modulo. Se un modulo contiene più pulsanti di invio, viene visualizzato solo il nome del pulsante su cui è stato fatto clic nella richiesta. Pertanto, in modo logico, se il nome di un pulsante viene visualizzato nella richiesta &mdash; o come indicato nel codice, se tale pulsante non è vuota &mdash; che è il pulsante di invio del modulo.

Il `&&` operatore significa "e" (AND logico). Pertanto l'intero `if` condizione è...

*Questa richiesta è una richiesta post (richiesta non primo)*  
  
 AND  
  
*Il* `buttonDelete` *pulsante è stato il pulsante di invio del modulo.*

Questo modulo (in effetti, questa pagina) contiene un solo pulsante, pertanto, il test per `buttonDelete` tecnicamente non è obbligatorio. Tuttavia, si sta per eseguire un'operazione che rimuove in modo permanente i dati. Pertanto, si desidera come assicurarsi che si sta eseguendo l'operazione solo quando l'utente ha richiesto in modo esplicito possibile. Si supponga, ad esempio, che espanso in un secondo momento questa pagina e aggiungere altri pulsanti. Anche in questo caso, il codice che elimina il filmato verrà eseguita solo se il `buttonDelete` è stato fatto clic sul pulsante.

Ad esempio il *EditMovie* pagina, ottenere l'ID del campo nascosto e quindi eseguire il comando SQL. La sintassi per la `Delete` istruzione:

`DELETE FROM table WHERE ID = value`

È importante includere la `WHERE` clausola e l'ID. Se si omette la clausola WHERE, *verranno eliminati tutti i record nella tabella*. Come si è visto, passare il valore ID per il comando SQL utilizzando un segnaposto.

## <a name="testing-the-movie-delete-process"></a>Verifica il processo di eliminazione del film

Ora è possibile eseguire il test. Eseguire il *filmati* pagina e fare clic su **eliminare** accanto a un filmato. Quando il *DeleteMovie* verrà visualizzata la pagina, fare clic su **eliminare film**.

![Elimina pagina film con il pulsante Elimina film evidenziato](deleting-data/_static/image4.png)

Quando si fa clic sul pulsante, il codice consente di eliminare i film e restituisce l'elenco di film. Non esiste è possibile cercare il film eliminato e confermare che è stato eliminato.

## <a name="coming-up-next"></a>Esercitazione seguente

L'esercitazione successiva viene illustrato come assegnare tutte le pagine nel sito aspetto e un layout comuni.

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a>Elenco completo per la pagina di film (aggiornata con collegamenti Elimina)

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a>Elenco completo per la pagina DeleteMovie

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a>Risorse aggiuntive

- [Introduzione alla programmazione Web ASP.NET utilizzando la sintassi Razor](../introducing-razor-syntax-c.md)
- [Istruzione SQL DELETE](http://www.w3schools.com/sql/sql_delete.asp) sul sito W3Schools

> [!div class="step-by-step"]
> [Precedente](updating-data.md)
> [Successivo](layouts.md)
