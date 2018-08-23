---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Aggiunge un nuovo campo al modello di filmato e tabella | Microsoft Docs
author: Rick-Anderson
description: 'Nota: Una versione aggiornata di questa esercitazione è disponibile qui che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro e molto più semplice da seguire e demo...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: 45e6de410266af9d7909cea6d03258d2db7cb240
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835808"
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a>Aggiunge un nuovo campo al modello di filmato e tabella
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > È disponibile una versione aggiornata di questa esercitazione [qui](../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e vengono illustrate altre funzionalità.


In questa sezione si userà migrazioni di Entity Framework Code First per eseguire la migrazione di alcune modifiche alle classi di modello, pertanto la modifica venga applicata nel database.

Per impostazione predefinita, quando si utilizza Code First di Entity Framework per creare automaticamente un database, come è stato fatto in precedenza in questa esercitazione, Code First consente di aggiungere una tabella nel database per rilevare se lo schema del database è sincronizzato con le classi del modello da che è stato generato. Se non sono sincronizzati, Entity Framework genera un errore. Questo rende più semplice individuare i problemi in fase di sviluppo che potrebbero altrimenti solo risultare (da errori sconosciuti) in fase di esecuzione.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Configurazione di migrazioni Code First per le modifiche al modello

Se si usa Visual Studio 2012, fare doppio clic il *Movies.mdf* file da Esplora soluzioni per aprire lo strumento di database. Visual Studio Express per Web verrà visualizzato Esplora Database, che Visual Studio 2012 mostrerà Esplora Server. Se si usa Visual Studio 2010, utilizzare Esplora oggetti di SQL Server.

Nello strumento di database (Database Explorer, Esplora Server o Esplora oggetti di SQL Server), fare clic sulla `MovieDBContext` e selezionare **eliminare** per eliminare il database di film.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

Passare a Esplora soluzioni. Fare clic con il pulsante destro sul *Movies.mdf* del file e selezionare **eliminare** per rimuovere il database di film.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

Compilare l'applicazione per assicurarsi che non siano presenti errori.

Dal **degli strumenti** menu, fare clic su **Library Package Manager** e quindi **Package Manager Console**.

![Aggiungere Pack Man](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

Nel **Console di gestione pacchetti** la finestra al `PM>` prompt dei comandi immettere "Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext".

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

Il **Enable-Migrations** comando (illustrato in precedenza) consente di creare un *Configuration.cs* file in una nuova *migrazioni* cartella.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

Visual Studio apre il *Configuration.cs* file. Sostituire il `Seed` metodo di *Configuration.cs* file con il codice seguente:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

Fare clic con il pulsante destro sulla riga rossa ondulata sotto `Movie` e selezionare **risolvere** quindi **usando** **mvcmovie. Models;**

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

In questo modo consente di aggiungere la seguente istruzione using:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> Codice prima le migrazioni chiamano il `Seed` metodo dopo ogni migrazione (vale a dire, la chiamata **update-database** nella Console di gestione pacchetti), e questo metodo aggiorna le righe che sono già state inserite o li inserisce se sono non esistono ancora.


**Premere CTRL + MAIUSC + B per compilare il progetto.** (La procedura seguente avrà esito negativo se il non vengono compilati a questo punto.)

Il passaggio successivo consiste nel creare un `DbMigration` classe per la migrazione iniziale. Questa migrazione a viene creato un nuovo database, che è per questo motivo hai eliminato il *movie.mdf* file in un passaggio precedente.

Nel **Console di gestione pacchetti** finestra, immettere il comando "add-migration Initial" per creare la migrazione iniziale. Il nome "Iniziale" è arbitrario e viene usato per denominare il file di migrazione creato.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

Migrazioni Code First consente di creare un altro file di classe nel *migrazioni* cartella (con il nome *{DateStamp}\_Initial.cs* ), e questa classe contiene codice che crea lo schema del database. Il nome del file di migrazione è pre-fissa con un timestamp nella Guida in linea con l'ordinamento. Esaminare i *{DateStamp}\_Initial.cs* file, contiene le istruzioni per creare la tabella di film per i database dei film. Quando si aggiorna il database nelle versioni precedenti, queste istruzioni *{DateStamp}\_Initial.cs* file verrà eseguito e creare lo schema di database. L'oggetto **Seed** metodo verrà eseguite per popolare il database con dati di test.

Nel **Console di gestione pacchetti**, immettere il comando "update-database" per creare il database ed eseguire il **Seed** (metodo).

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

Se si verifica un errore che indica una tabella esiste già e non può essere creata, è probabilmente perché è stata eseguita l'applicazione dopo che è stato eliminato il database e prima di eseguire `update-database`. In tal caso, eliminare il *Movies.mdf* nuovamente file e ripetere il `update-database` comando. Se viene ancora visualizzato un errore, eliminare la cartella migrations e il contenuto, iniziare con le istruzioni nella parte superiore della pagina (eliminazione che è il *Movies.mdf* file, quindi procedere a Enable-Migrations).

Eseguire l'applicazione e passare al */Movies* URL. I dati di seeding viene visualizzati.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Aggiunta di una proprietà Rating al modello Movie

Iniziare aggiungendo una nuova `Rating` proprietà esistente `Movie` classe. Aprire il *Models\Movie.cs* file e aggiungere il `Rating` proprietà simile alla seguente:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

L'intero `Movie` classe avrà l'aspetto simile al codice seguente:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

Compilare l'applicazione usando il **compilare** &gt; **compilare film** menu comando o premendo CTRL-MAIUSC-B.

Ora che è stato aggiornato il `Model` (classe), è anche necessario aggiornare il *\Views\Movies\Index.cshtml* e *\Views\Movies\Create.cshtml* consente di visualizzare i modelli per visualizzare il nuovo `Rating`proprietà nella vista del browser.

Aprire il<em>\Views\Movies\Index.cshtml</em> file e aggiungere un `<th>Rating</th>` intestazione di colonna subito dopo il <strong>prezzo</strong> colonna. Aggiungere quindi una `<td>` colonna verso la fine del modello per eseguire il rendering di `@item.Rating` valore. Ecco quali aggiornato <em>index. cshtml</em> modello di visualizzazione è simile a:

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

Successivamente, aprire il *\Views\Movies\Create.cshtml* file e aggiungere il markup seguente verso la fine del form. Si esegue il rendering di una casella di testo in modo che sia possibile specificare una classificazione uguale a quando viene creato un nuovo film.

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

A questo punto è stato aggiornato il codice dell'applicazione per supportare la nuova `Rating` proprietà.

Ora eseguire l'applicazione e passare al */Movies* URL. Quando si esegue questa operazione, tuttavia, si noterà uno dei seguenti errori:

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

Viene visualizzato questo errore perché aggiornato `Movie` classe di modello nell'applicazione ora è diverso rispetto allo schema del `Movie` tabella del database esistente. Nella tabella del database non è presente una colonna `Rating`.


Per correggere questo errore, esistono alcuni approcci:

1. Fare in modo che Entity Framework elimini e crei di nuovo automaticamente il database in base al nuovo schema di classi del modello. Questo approccio è molto utile quando si esegue lo sviluppo attivo in un database di test. Consente di migliorare rapidamente lo schema di modello e il database insieme. Lo svantaggio, tuttavia, è che si perdono i dati esistenti nel database, in modo che è *non* vuole usare questo approccio in un database di produzione. Usare un inizializzatore per inizializzare automaticamente un database con dati di test è spesso un modo produttivo per sviluppare un'applicazione. Per altre informazioni sugli inizializzatori di database di Entity Framework, vedere di Tom Dykstra [esercitazione su ASP.NET MVC o Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Modificare esplicitamente lo schema del database esistente in modo che corrisponda alle classi del modello. Il vantaggio di questo approccio è che i dati vengono mantenuti. È possibile apportare questa modifica manualmente o creando uno script di modifica del database.
3. Usare Migrazioni Code First per aggiornare lo schema del database.


Per questa esercitazione si userà Migrazioni Code First.

Aggiornare il metodo di inizializzazione in modo che fornisca un valore per la nuova colonna. Aprire il file Migrations\Configuration.cs e aggiungere un campo di classificazione per ogni oggetto del film.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

Compilare la soluzione e quindi aprire il **Console di gestione pacchetti** finestra e immettere il comando seguente:

`add-migration AddRatingMig`

Il `add-migration` comando indica al framework di migrazione per esaminare il modello di filmato corrente con lo schema di film DB corrente e creare il codice necessario per eseguire la migrazione del database al nuovo modello. Il AddRatingMig è arbitrario e viene usato per denominare il file di migrazione. È utile usare un nome significativo per il passaggio della migrazione.

Al termine di questo comando, Visual Studio apre il file di classe che definisce la nuova `DbMIgration` classe derivata e il `Up` metodo è possibile visualizzare il codice che crea la nuova colonna.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

Compilare la soluzione e quindi immettere il comando "update-database" nel **Console di gestione pacchetti** finestra.

L'immagine seguente mostra l'output nel **Console di gestione pacchetti** finestra (l'indicatore di data anteponendo AddRatingMig sarà diverso.)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

Eseguire nuovamente l'applicazione e passare all'URL /Movies. È possibile visualizzare il nuovo campo di classificazione.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

Scegliere il **Crea nuovo** collegamento per aggiungere un nuovo film. Si noti che è possibile aggiungere una classificazione.

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

Scegliere **Crea**. Questo nuovo film, tra cui la classificazione, ora viene visualizzata nell'elenco di film:

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

È necessario aggiungere anche il `Rating` campo per la modifica, dettagli e SearchIndex modelli di visualizzazione.

È possibile immettere il comando "update-database" nel **Console di gestione pacchetti** finestra nuovamente e senza apportare modifiche al sarebbe Impossibile stabilire la, lo schema corrisponde al modello.

In questa sezione è stato illustrato come è possibile modificare gli oggetti modello e sincronizzare il database con le modifiche. Si è appreso anche un modo per popolare un database appena creato con dati di esempio in modo che è possibile provare gli scenari. Successivamente, diamo un'occhiata a come è possibile aggiungere più completa della logica di convalida alle classi di modello e abilitare alcune regole di business possano essere applicate.

> [!div class="step-by-step"]
> [Precedente](examining-the-edit-methods-and-edit-view.md)
> [Successivo](adding-validation-to-the-model.md)
