---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: Aggiunge un nuovo campo | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 87bb2c5f64e714268f5e2631b44fbb8a93a6a4b6
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578090"
---
<a name="adding-a-new-field"></a>Aggiunta di un nuovo campo
====================
da [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](sample/code-location.md)]

In questa sezione si userà migrazioni di Entity Framework Code First per eseguire la migrazione di alcune modifiche alle classi di modello, pertanto la modifica venga applicata nel database.

Per impostazione predefinita, quando si utilizza Code First di Entity Framework per creare automaticamente un database, come è stato fatto in precedenza in questa esercitazione, Code First consente di aggiungere una tabella nel database per rilevare se lo schema del database è sincronizzato con le classi del modello da che è stato generato. Se non sono sincronizzati, Entity Framework genera un errore. Questo rende più semplice individuare i problemi in fase di sviluppo che potrebbero altrimenti solo risultare (da errori sconosciuti) in fase di esecuzione.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Configurazione di migrazioni Code First per le modifiche al modello

Passare a Esplora soluzioni. Fare clic con il pulsante destro sul *Movies.mdf* del file e selezionare **eliminare** per rimuovere il database di film. Se non viene visualizzato il *Movies.mdf* del file, fare clic sui **Mostra tutti i file** illustrato di seguito nel contorno rosso sull'icona.

![](adding-a-new-field/_static/image1.png)

Compilare l'applicazione per assicurarsi che non siano presenti errori.

Dal **degli strumenti** menu, fare clic su **Gestione pacchetti NuGet** e quindi **Package Manager Console**.

![Aggiungere Pack Man](adding-a-new-field/_static/image2.png)

Nel **Console di gestione pacchetti** la finestra al `PM>` prompt dei comandi immettere

Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext

![](adding-a-new-field/_static/image3.png)

Il **Enable-Migrations** comando (illustrato in precedenza) consente di creare un *Configuration.cs* file in una nuova *migrazioni* cartella.

![](adding-a-new-field/_static/image4.png)

Visual Studio apre il *Configuration.cs* file. Sostituire il `Seed` metodo di *Configuration.cs* file con il codice seguente:

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

Passare il mouse sulla riga rossa ondulata sotto `Movie` e fare clic su `Show Potential Fixes` e quindi fare clic su **usando** **mvcmovie. Models;**

![](adding-a-new-field/_static/image5.png)

In questo modo consente di aggiungere la seguente istruzione using:

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE]
> 
> Codice prima le migrazioni chiamano il `Seed` metodo dopo ogni migrazione (vale a dire, la chiamata **update-database** nella Console di gestione pacchetti), e questo metodo aggiorna le righe che sono già state inserite o li inserisce se sono non esistono ancora.
> 
> Il [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metodo nel codice seguente esegue un'operazione "upsert":
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> Poiché il [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metodo viene eseguito con tutte le migrazioni, è possibile inserire solo i dati, perché si sta provando ad aggiungere righe saranno già presenti dopo la migrazione prima che crea il database. Il "[upsert](http://en.wikipedia.org/wiki/Upsert)" operazione impedisce gli errori che accadrebbe se si prova a inserire una riga già esistente, ma esegue l'override di eventuali modifiche ai dati apportate durante il test dell'applicazione. I dati di test in alcune tabelle è possibile evitare di raggiungere tale obiettivo: in alcuni casi quando si modificano i dati durante il test per le proprie modifiche permangono dopo gli aggiornamenti del database. In questo caso si vuole eseguire un'operazione di inserimento condizionale: inserire una riga solo se non esiste già.   
> 
> Il primo parametro passato per il [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metodo consente di specificare la proprietà da utilizzare per verificare se esiste già una riga. Per i dati di film di test che si desidera fornire, il `Title` proprietà può essere utilizzata per questo scopo poiché ogni titolo nell'elenco è univoco:
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> Questo codice presuppone che i titoli siano univoci. Se si aggiunge manualmente un titolo duplicato, si otterrà l'eccezione seguente la volta successiva che si esegue una migrazione.   
> 
>  *La sequenza contiene più di un elemento*  
> 
> Per altre informazioni sul [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metodo, vedere [prestare attenzione con Entity Framework 4.3 AddOrUpdate metodo](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)...


**Premere CTRL + MAIUSC + B per compilare il progetto.** (La procedura seguente avrà esito negativo se non crei a questo punto.)

Il passaggio successivo consiste nel creare un `DbMigration` classe per la migrazione iniziale. Questa migrazione crea un nuovo database, che è per questo motivo hai eliminato il *movie.mdf* file in un passaggio precedente.

Nel **Console di gestione pacchetti** finestra, immettere il comando `add-migration Initial` per creare la migrazione iniziale. Il nome "Iniziale" è arbitrario e viene usato per denominare il file di migrazione creato.

![](adding-a-new-field/_static/image6.png)

Migrazioni Code First consente di creare un altro file di classe nel *migrazioni* cartella (con il nome *{DateStamp}\_Initial.cs* ), e questa classe contiene codice che crea lo schema del database. Il nome del file di migrazione è pre-fissa con un timestamp nella Guida in linea con l'ordinamento. Esaminare i *{DateStamp}\_Initial.cs* file, contiene le istruzioni per creare il `Movies` tabella per il database dei film. Quando si aggiorna il database nelle versioni precedenti, queste istruzioni *{DateStamp}\_Initial.cs* file verrà eseguito e creare lo schema di database. L'oggetto **Seed** metodo verrà eseguite per popolare il database con dati di test.

Nel **Console di gestione pacchetti**, immettere il comando `update-database` per creare il database ed eseguire il `Seed` (metodo).

![](adding-a-new-field/_static/image7.png)

Se si verifica un errore che indica una tabella esiste già e non può essere creata, è probabilmente perché è stata eseguita l'applicazione dopo che è stato eliminato il database e prima di eseguire `update-database`. In tal caso, eliminare il *Movies.mdf* nuovamente file e ripetere il `update-database` comando. Se viene ancora visualizzato un errore, eliminare la cartella migrations e il contenuto, iniziare con le istruzioni nella parte superiore della pagina (eliminazione che è il *Movies.mdf* file, quindi procedere a Enable-Migrations). Se viene ancora visualizzato un messaggio di errore, aprire Esplora oggetti di SQL Server e rimuovere il database dall'elenco.

Eseguire l'applicazione e passare al */Movies* URL. I dati di seeding viene visualizzati.

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Aggiunta di una proprietà Rating al modello Movie

Iniziare aggiungendo una nuova `Rating` proprietà esistente `Movie` classe. Aprire il *Models\Movie.cs* file e aggiungere il `Rating` proprietà simile alla seguente:

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

L'intero `Movie` classe avrà l'aspetto simile al codice seguente:

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

Compilare l'applicazione (Ctrl + MAIUSC + B).

Poiché è stato aggiunto un nuovo campo per il `Movie` (classe), è anche necessario aggiornare l'associazione *all'elenco elementi consentiti* in modo da includere questa nuova proprietà. Aggiornamento di `bind` dell'attributo per `Create` e `Edit` metodi di azione per includere il `Rating` proprietà:

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

È necessario anche aggiornare i modelli di vista per visualizzare, creare e modificare la nuova proprietà `Rating` nella vista del browser.

Aprire il *\Views\Movies\Index.cshtml* file e aggiungere un `<th>Rating</th>` intestazione di colonna subito dopo il **prezzo** colonna. Aggiungere quindi una `<td>` colonna verso la fine del modello per eseguire il rendering di `@item.Rating` valore. Ecco quali aggiornato *index. cshtml* modello di visualizzazione è simile a:

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

Successivamente, aprire il *\Views\Movies\Create.cshtml* file e aggiungere il `Rating` campo con il markup seguente highlighed. Si esegue il rendering di una casella di testo in modo che sia possibile specificare una classificazione uguale a quando viene creato un nuovo film.

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

A questo punto è stato aggiornato il codice dell'applicazione per supportare la nuova `Rating` proprietà.

Eseguire l'applicazione e passare al */Movies* URL. Quando si esegue questa operazione, tuttavia, si noterà uno dei seguenti errori:

![](adding-a-new-field/_static/image9.png)  
  
Il modello supporta il contesto 'MovieDBContext' è stato modificato dal momento della creazione del database. È consigliabile usare migrazioni Code First per aggiornare il database (https://go.microsoft.com/fwlink/?LinkId=238269).

![](adding-a-new-field/_static/image10.png)

Viene visualizzato questo errore perché aggiornato `Movie` classe di modello nell'applicazione ora è diverso rispetto allo schema del `Movie` tabella del database esistente. Nella tabella del database non è presente una colonna `Rating`.


Per correggere questo errore, esistono alcuni approcci:

1. Fare in modo che Entity Framework elimini e crei di nuovo automaticamente il database in base al nuovo schema di classi del modello. Questo approccio è molto utile nelle prime fasi del ciclo di sviluppo in una fase attiva di sviluppo di un database di test e consente di migliorare rapidamente lo schema del modello e il database insieme. Lo svantaggio, tuttavia, è che si perdono i dati esistenti nel database, in modo che è *non* vuole usare questo approccio in un database di produzione. Un modo efficace per sviluppare un'applicazione consiste nell'inizializzare automaticamente un database con dati di test usando un inizializzatore. Per altre informazioni sugli inizializzatori di database di Entity Framework, vedere [esercitazione su ASP.NET MVC o Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Modificare esplicitamente lo schema del database esistente in modo che corrisponda alle classi del modello. Il vantaggio di questo approccio è che i dati vengono mantenuti. È possibile apportare questa modifica manualmente o creando uno script di modifica del database.
3. Usare Migrazioni Code First per aggiornare lo schema del database.


Per questa esercitazione si userà Migrazioni Code First.

Aggiornare il metodo di inizializzazione in modo che fornisca un valore per la nuova colonna. Aprire il file Migrations\Configuration.cs e aggiungere un campo di classificazione per ogni oggetto del film.

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

Compilare la soluzione e quindi aprire il **Console di gestione pacchetti** finestra e immettere il comando seguente:

`add-migration Rating`

Il `add-migration` comando indica al framework di migrazione per esaminare il modello di filmato corrente con lo schema di film DB corrente e creare il codice necessario per eseguire la migrazione del database al nuovo modello. Il nome *Rating* è arbitrario e viene usato per denominare il file di migrazione. È utile usare un nome significativo per il passaggio della migrazione.

Al termine di questo comando, Visual Studio apre il file di classe che definisce la nuova `DbMigration` classe derivata e il `Up` metodo è possibile visualizzare il codice che crea la nuova colonna.

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

Compilare la soluzione e quindi immettere il `update-database` comando il **Console di gestione pacchetti** finestra.

L'immagine seguente mostra l'output nel **Console di gestione pacchetti** finestra (il timbro data anteponendo *Rating* sarà diverso.)

![](adding-a-new-field/_static/image11.png)

Eseguire nuovamente l'applicazione e passare all'URL /Movies. È possibile visualizzare il nuovo campo di classificazione.

![](adding-a-new-field/_static/image12.png)

Scegliere il **Crea nuovo** collegamento per aggiungere un nuovo film. Si noti che è possibile aggiungere una classificazione.

![7_CreateRioII](adding-a-new-field/_static/image13.png)

Scegliere **Crea**. Questo nuovo film, tra cui la classificazione, ora viene visualizzata nell'elenco di film:

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

Ora che il progetto usi le migrazioni, non è necessario eliminare il database quando si aggiunge un nuovo campo o in caso contrario, aggiornare lo schema. Nella sezione successiva, si sarà apportare altre modifiche allo schema e usare le migrazioni per aggiornare il database.

È necessario aggiungere anche il `Rating` campo per i modelli di visualizzazione di modifica, dettagli e Delete.

È possibile immettere il comando "update-database" nel **Console di gestione pacchetti** nuovamente finestra e nessun codice di migrazione verrebbe eseguito, perché lo schema corrisponde al modello. In esecuzione "update-database" viene però eseguito il `Seed` metodo anche in questo caso e se è stato modificato, i dati di valore di inizializzazione, le modifiche andranno perse perché il `Seed` dati esegue l'Upsert (metodo). Altre informazioni, vedere la `Seed` metodo nella di Tom Dykstra popolare [esercitazione su ASP.NET MVC o Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

In questa sezione è stato illustrato come è possibile modificare gli oggetti modello e sincronizzare il database con le modifiche. Si è appreso anche un modo per popolare un database appena creato con dati di esempio in modo che è possibile provare gli scenari. Questo è solo una rapida introduzione alle Code First, vedere [creazione di un modello di dati di Entity Framework per un'applicazione ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) per un'esercitazione più completa sull'argomento. Successivamente, diamo un'occhiata a come è possibile aggiungere più completa della logica di convalida alle classi di modello e abilitare alcune regole di business possano essere applicate.

> [!div class="step-by-step"]
> [Precedente](adding-search.md)
> [Successivo](adding-validation.md)
