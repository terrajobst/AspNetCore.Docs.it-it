---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Aggiunta di un nuovo campo al modello di film e di tabella | Documenti Microsoft
author: Rick-Anderson
description: "Nota: Una versione aggiornata di questa esercitazione è disponibile qui che utilizza ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice seguire e demo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: 0c094c4d4c99702a5b513717126872a254ca3e5a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a>Aggiunta di un nuovo campo al modello di film e di tabella
====================
Da [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > È disponibile una versione aggiornata di questa esercitazione [qui](../../getting-started/introduction/getting-started.md) che utilizza ASP.NET MVC 5 e Visual Studio 2013. È molto più semplice da seguire, più sicuro e vengono illustrate altre funzionalità.


In questa sezione si userà le migrazioni di Entity Framework Code First per eseguire la migrazione di alcune modifiche alle classi di modello, pertanto la modifica venga applicata al database.

Per impostazione predefinita, quando si utilizza Code First di Entity Framework per creare automaticamente un database, come in precedenza in questa esercitazione, il primo codice aggiunge una tabella nel database per rilevare se lo schema del database è sincronizzato con le classi di modello da che è stato generato. Se non sono sincronizzati, Entity Framework genera un errore. Questo rende più semplice individuare i problemi in fase di sviluppo che potrebbe essere in caso contrario solo (da errori oscuri) in fase di esecuzione.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Configurazione di migrazioni Code First per le modifiche al modello

Se si utilizza Visual Studio 2012, fare doppio clic il *Movies.mdf* file da Esplora soluzioni per aprire lo strumento di database. Visual Studio Express per Web verrà visualizzato Esplora Database, che Visual Studio 2012 verranno visualizzati Esplora Server. Se si utilizza Visual Studio 2010, utilizzare Esplora oggetti di SQL Server.

Lo strumento di database (Database Explorer, Esplora Server o Esplora oggetti di SQL Server), fare clic sul `MovieDBContext` e selezionare **eliminare** per eliminare il database di film.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

Tornare alla Esplora soluzioni. Fare clic con il pulsante destro sul *Movies.mdf* file e selezionare **eliminare** per rimuovere il database di film.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

Compilare l'applicazione per assicurarsi che non siano presenti errori.

Dal **strumenti** menu, fare clic su **Gestione pacchetti libreria** e quindi **Package Manager Console**.

![Aggiungere i Management Pack](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

Nel **Package Manager Console** finestra il `PM>` prompt dei comandi immettere "Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext".

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

Il **Enable-Migrations** comando (illustrato in precedenza) crea un *Configuration.cs* file in un nuovo *migrazioni* cartella.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

Visual Studio apre il *Configuration.cs* file. Sostituire il `Seed` metodo il *Configuration.cs* file con il codice seguente:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

Fare clic con il pulsante destro sulla riga rossa ondulata sotto `Movie` e selezionare **risolvere** quindi **utilizzando** **MvcMovie.Models;**

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

In questo modo consente di aggiungere la seguente istruzione using:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> Codice viene chiamato prima di migrazioni di `Seed` metodo dopo ogni migrazione (la chiamata al metodo **update-database** nella Console di gestione pacchetti), e questo metodo aggiorna le righe che sono già state inserite o li inserisce se sono non esistono ancora.


**Premere CTRL-MAIUSC-B per compilare il progetto.** (La procedura seguente avrà esito negativo se il non vengono compilati a questo punto.)

Il passaggio successivo consiste nel creare un `DbMigration` classe per la migrazione iniziale. La migrazione a Crea un nuovo database, perché è eliminato il *movie.mdf* file in un passaggio precedente.

Nel **Package Manager Console** finestra, immettere il comando "migrazione aggiungere iniziale" per creare la migrazione iniziale. Il nome "Iniziale" è arbitrario e viene utilizzato per denominare il file di migrazione creato.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

Migrazioni Code First consente di creare un altro file di classe di *migrazioni* cartella (con il nome *{DateStamp}\_Initial.cs* ), e questa classe contiene codice che crea lo schema del database. Il nome del file di migrazione è pre-fissa con un timestamp per agevolare l'ordinamento. Esaminare il *{DateStamp}\_Initial.cs* file, contiene le istruzioni per creare la tabella di filmati per il database di film. Quando si aggiorna il database nelle istruzioni seguenti, *{DateStamp}\_Initial.cs* file verrà eseguito e creare il lo schema di database. Il **valore di inizializzazione** metodo verrà eseguito per popolare il database con dati di test.

Nel **Package Manager Console**, immettere il comando "update-database" per creare il database ed eseguire il **valore di inizializzazione** metodo.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

Se si verifica un errore che indica una tabella esiste già e non può essere creata, è probabilmente perché è stata eseguita l'applicazione dopo l'eliminazione di database e prima di eseguire `update-database`. In tal caso, eliminare il *Movies.mdf* file nuovo, quindi ripetere il `update-database` comando. Se viene ancora visualizzato un errore, eliminare la cartella migrations e il contenuto, quindi iniziare con le istruzioni nella parte superiore della pagina (che è l'eliminazione di *Movies.mdf* file, quindi procedere con il comando Enable-Migrations).

Eseguire l'applicazione e passare il */Movies* URL. Visualizzazione dei dati del valore di inizializzazione.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Aggiunta di una proprietà Rating al modello Movie

Per iniziare, aggiungere un nuovo `Rating` proprietà esistente `Movie` classe. Aprire il *Models\Movie.cs* file e aggiungere il `Rating` proprietà simile alla seguente:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

L'intero `Movie` classe ora ha un aspetto simile al codice seguente:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

Compilare l'applicazione tramite il **compilare** &gt; **compilare film** menu comando oppure premendo CTRL-MAIUSC-B.

Ora che è stato aggiornato il `Model` (classe), è anche necessario aggiornare il *\Views\Movies\Index.cshtml* e *\Views\Movies\Create.cshtml* consente di visualizzare i modelli per visualizzare il nuovo `Rating`proprietà nella vista del browser.

Aprire il*\Views\Movies\Index.cshtml* file e aggiungere un `<th>Rating</th>` intestazione di colonna subito dopo il **prezzo** colonna. Aggiungere quindi una `<td>` colonna verso la fine del modello per il rendering di `@item.Rating` valore. Di seguito è riportato l'aggiornato *cshtml* modello di visualizzazione è simile a:

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

Aprire quindi il *\Views\Movies\Create.cshtml* file e aggiungere il markup seguente verso la fine del form. Si esegue il rendering di una casella di testo in modo che quando viene creato un nuovo filmato, è possibile specificare una classificazione.

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

Ora è stato aggiornato il codice dell'applicazione per supportare la nuova `Rating` proprietà.

Eseguire l'applicazione e passare al */Movies* URL. Quando si esegue questa operazione, tuttavia, verrà visualizzato uno dei seguenti errori:

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

Viene visualizzato questo errore perché l'aggiornamento `Movie` classe del modello nell'applicazione è diverso rispetto allo schema di ora il `Movie` tabella del database esistente. Nella tabella del database non è presente una colonna `Rating`.


Per correggere questo errore, esistono alcuni approcci:

1. Fare in modo che Entity Framework elimini e crei di nuovo automaticamente il database in base al nuovo schema di classi del modello. Questo approccio è molto utile durante la fase di sviluppo attivo su un database di test. Consente di rapidamente evolvere lo schema del modello e il database insieme. Lo svantaggio, tuttavia, è che si perdono i dati esistenti nel database, in modo si *non* per usare questo approccio in un database di produzione. Utilizzando un inizializzatore per inizializzare automaticamente un database con dati di test è spesso un modo produttivo per sviluppare un'applicazione. Per ulteriori informazioni sugli inizializzatori di database di Entity Framework, vedere di Tom Dykstra [esercitazione ASP.NET MVC/Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Modificare esplicitamente lo schema del database esistente in modo che corrisponda alle classi del modello. Il vantaggio di questo approccio è che i dati vengono mantenuti. È possibile apportare questa modifica manualmente o creando uno script di modifica del database.
3. Usare Migrazioni Code First per aggiornare lo schema del database.


Per questa esercitazione si userà Migrazioni Code First.

Aggiornare il metodo di inizializzazione in modo che fornisca un valore per la nuova colonna. Aprire il file Migrations\Configuration.cs e aggiungere un campo di classificazione per ogni oggetto del film.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

Compilare la soluzione e quindi aprire il **Package Manager Console** finestra e immettere il comando seguente:

`add-migration AddRatingMig`

Il `add-migration` comando indica al framework di migrazione per esaminare il modello di film corrente con lo schema di film DB corrente e creare il codice necessario per eseguire la migrazione del database al nuovo modello. Il AddRatingMig è arbitrario e viene utilizzato per denominare il file di migrazione. È consigliabile utilizzare un nome significativo per il passaggio della migrazione.

Al termine di questo comando, Visual Studio apre il file di classe che definisce la nuova `DbMIgration` classe derivata e il `Up` metodo è possibile visualizzare il codice che crea la nuova colonna.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

Compilare la soluzione e quindi immettere il comando "Aggiorna database" di **Package Manager Console** finestra.

La figura seguente mostra l'output di **Console di gestione pacchetti** finestra (l'indicatore di data anteponendo AddRatingMig saranno diverso.)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

Eseguire nuovamente l'applicazione e passare all'URL /Movies. È possibile visualizzare il nuovo campo di classificazione.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

Fare clic su di **Crea nuovo** collegamento per aggiungere un nuovo film. Si noti che è possibile aggiungere una classificazione.

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

Scegliere **Crea**. Il nuovo filmato, tra cui la valutazione, viene inserito nei film elenco:

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

È inoltre necessario aggiungere il `Rating` campo per la modifica, dettagli e SearchIndex modelli di visualizzazione.

È possibile immettere il comando "Aggiorna database" di **Package Manager Console** nuovamente finestra e modifiche non verranno apportate, perché lo schema corrisponde al modello.

In questa sezione è stato illustrato come è possibile modificare gli oggetti del modello e sincronizzare il database con le modifiche. È stato inoltre un modo per popolare un database appena creato con dati di esempio in modo è possibile provare gli scenari. Successivamente, si esaminerà come è possibile aggiungere la logica di convalida più completa per le classi di modello e abilitare alcune regole di business da applicare.

>[!div class="step-by-step"]
[Precedente](examining-the-edit-methods-and-edit-view.md)
[Successivo](adding-validation-to-the-model.md)
