---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: Aggiunta di un nuovo campo | Documenti Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 0dac798eba586cdcc232cedd262e610b954004df
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-new-field"></a>Aggiunta di un nuovo campo
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

In questa sezione si userà le migrazioni di Entity Framework Code First per eseguire la migrazione di alcune modifiche alle classi di modello, pertanto la modifica venga applicata al database.

Per impostazione predefinita, quando si utilizza Code First di Entity Framework per creare automaticamente un database, come in precedenza in questa esercitazione, il primo codice aggiunge una tabella nel database per rilevare se lo schema del database è sincronizzato con le classi di modello da che è stato generato. Se non sono sincronizzati, Entity Framework genera un errore. Questo rende più semplice individuare i problemi in fase di sviluppo che potrebbe essere in caso contrario solo (da errori oscuri) in fase di esecuzione.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Configurazione di migrazioni Code First per le modifiche al modello

Passare a Esplora soluzioni. Fare clic con il pulsante destro sul *Movies.mdf* file e selezionare **eliminare** per rimuovere il database di film. Se non viene visualizzato il *Movies.mdf* file, fare clic su di **Mostra tutti i file** icona visualizzata di sotto della struttura di colore rosso.

![](adding-a-new-field/_static/image1.png)

Compilare l'applicazione per assicurarsi che non siano presenti errori.

Dal **strumenti** menu, fare clic su **Gestione pacchetti NuGet** e quindi **Package Manager Console**.

![Aggiungere i Management Pack](adding-a-new-field/_static/image2.png)

Nel **Package Manager Console** finestra il `PM>` prompt dei comandi immettere

Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext

![](adding-a-new-field/_static/image3.png)

Il **Enable-Migrations** comando (illustrato in precedenza) crea un *Configuration.cs* file in un nuovo *migrazioni* cartella.

![](adding-a-new-field/_static/image4.png)

Visual Studio apre il *Configuration.cs* file. Sostituire il `Seed` metodo il *Configuration.cs* file con il codice seguente:

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

Passare il mouse sulla riga rossa ondulata sotto `Movie` e fare clic su `Show Potential Fixes` e quindi fare clic su **utilizzando** **MvcMovie.Models;**

![](adding-a-new-field/_static/image5.png)

In questo modo consente di aggiungere la seguente istruzione using:

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE]
> 
> Codice viene chiamato prima di migrazioni di `Seed` metodo dopo ogni migrazione (la chiamata al metodo **update-database** nella Console di gestione pacchetti), e questo metodo aggiorna le righe che sono già state inserite o li inserisce se sono non esistono ancora.
> 
> Il [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metodo nel codice seguente esegue un'operazione "upsert":
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> Poiché il [valore di inizializzazione](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) metodo viene eseguito con ogni tipo di migrazione, è possibile inserire solo dati, in quanto le righe che si sta tentando di aggiungere già sarà disponibile dopo la migrazione prima che crea il database. Il "[upsert](http://en.wikipedia.org/wiki/Upsert)" operazione impedisce gli errori che accadrebbe se si tenta di inserire una riga già esistente, ma esegue l'override di qualsiasi modifica ai dati apportate durante il test dell'applicazione. I dati di test in alcune tabelle è possibile evitare di raggiungere tale obiettivo: in alcuni casi quando si modificano i dati durante il test le proprie modifiche rimangano dopo gli aggiornamenti del database. In questo caso si desidera eseguire un'operazione di inserimento condizionale: inserire una riga solo se non esiste già.   
> 
> Il primo parametro passato per il [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metodo consente di specificare la proprietà da utilizzare per verificare se esiste già una riga. Per i dati di test filmato che si desidera fornire, il `Title` proprietà può essere utilizzata per questo scopo poiché ogni libro nell'elenco è univoco:
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> Questo codice si presuppone che i titoli siano univoci. Se si aggiunge manualmente un titolo duplicato, si otterrà la seguente eccezione alla successiva che si eseguire la migrazione.   
> 
>  *La sequenza contiene più di un elemento*  
> 
> Per ulteriori informazioni sul [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) metodo, vedere [prestare attenzione con EF 4.3 AddOrUpdate metodo](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)...


**Premere CTRL-MAIUSC-B per compilare il progetto.** (La procedura seguente avrà esito negativo se a questo punto.)

Il passaggio successivo consiste nel creare un `DbMigration` classe per la migrazione iniziale. Questa migrazione crea un nuovo database, perché è eliminato il *movie.mdf* file in un passaggio precedente.

Nel **Package Manager Console** finestra, immettere il comando `add-migration Initial` per creare la migrazione iniziale. Il nome "Iniziale" è arbitrario e viene utilizzato per denominare il file di migrazione creato.

![](adding-a-new-field/_static/image6.png)

Migrazioni Code First consente di creare un altro file di classe di *migrazioni* cartella (con il nome *{DateStamp}\_Initial.cs* ), e questa classe contiene codice che crea lo schema del database. Il nome del file di migrazione è pre-fissa con un timestamp per agevolare l'ordinamento. Esaminare il *{DateStamp}\_Initial.cs* file, contiene le istruzioni per creare il `Movies` tabella per il database di film. Quando si aggiorna il database nelle istruzioni seguenti, *{DateStamp}\_Initial.cs* file verrà eseguito e creare lo schema di database. Il **valore di inizializzazione** metodo verrà eseguito per popolare il database con dati di test.

Nel **Package Manager Console**, immettere il comando `update-database` per creare il database ed eseguire il `Seed` metodo.

![](adding-a-new-field/_static/image7.png)

Se si verifica un errore che indica una tabella esiste già e non può essere creata, è probabilmente perché è stata eseguita l'applicazione dopo l'eliminazione di database e prima di eseguire `update-database`. In tal caso, eliminare il *Movies.mdf* file nuovo, quindi ripetere il `update-database` comando. Se viene ancora visualizzato un errore, eliminare la cartella migrations e il contenuto, quindi iniziare con le istruzioni nella parte superiore della pagina (che è l'eliminazione di *Movies.mdf* file, quindi procedere con il comando Enable-Migrations). Se viene visualizzato ancora un se, aprire Esplora oggetti di SQL Server e rimuovere il database dall'elenco.

Eseguire l'applicazione e passare il */Movies* URL. Visualizzazione dei dati del valore di inizializzazione.

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Aggiunta di una proprietà Rating al modello Movie

Per iniziare, aggiungere un nuovo `Rating` proprietà esistente `Movie` classe. Aprire il *Models\Movie.cs* file e aggiungere il `Rating` proprietà simile alla seguente:

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

L'intero `Movie` classe ora ha un aspetto simile al codice seguente:

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

Compilare l'applicazione (Ctrl + MAIUSC + B).

Poiché hai aggiunto un nuovo campo di `Movie` (classe), è inoltre necessario aggiornare l'associazione *elenco approvato* in modo che sia inclusa la nuova proprietà. Aggiornamento di `bind` attributo per `Create` e `Edit` metodi di azione per includere il `Rating` proprietà:

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

È necessario anche aggiornare i modelli di vista per visualizzare, creare e modificare la nuova proprietà `Rating` nella vista del browser.

Aprire il *\Views\Movies\Index.cshtml* file e aggiungere un `<th>Rating</th>` intestazione di colonna subito dopo il **prezzo** colonna. Aggiungere quindi una `<td>` colonna verso la fine del modello per il rendering di `@item.Rating` valore. Di seguito è riportato l'aggiornato *cshtml* modello di visualizzazione è simile a:

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

Aprire quindi il *\Views\Movies\Create.cshtml* file e aggiungere il `Rating` campo con il seguente markup highlighed. Si esegue il rendering di una casella di testo in modo che quando viene creato un nuovo filmato, è possibile specificare una classificazione.

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

Ora è stato aggiornato il codice dell'applicazione per supportare la nuova `Rating` proprietà.

Eseguire l'applicazione e passare il */Movies* URL. Quando si esegue questa operazione, tuttavia, verrà visualizzato uno dei seguenti errori:

![](adding-a-new-field/_static/image9.png)  
  
Il modello di esecuzione del backup il contesto 'MovieDBContext' è stato modificato dopo la creazione del database. Provare a utilizzare migrazioni Code First per aggiornare il database (https://go.microsoft.com/fwlink/?LinkId=238269).

![](adding-a-new-field/_static/image10.png)

Viene visualizzato questo errore perché l'aggiornamento `Movie` classe del modello nell'applicazione è diverso rispetto allo schema di ora il `Movie` tabella del database esistente. Nella tabella del database non è presente una colonna `Rating`.


Per correggere questo errore, esistono alcuni approcci:

1. Fare in modo che Entity Framework elimini e crei di nuovo automaticamente il database in base al nuovo schema di classi del modello. Questo approccio è molto utile nelle prime fasi del ciclo di sviluppo in una fase attiva di sviluppo di un database di test e consente di migliorare rapidamente lo schema del modello e il database insieme. Lo svantaggio, tuttavia, è che si perdono i dati esistenti nel database, in modo si *non* per usare questo approccio in un database di produzione. Un modo efficace per sviluppare un'applicazione consiste nell'inizializzare automaticamente un database con dati di test usando un inizializzatore. Per ulteriori informazioni sugli inizializzatori di database di Entity Framework, vedere [esercitazione ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Modificare esplicitamente lo schema del database esistente in modo che corrisponda alle classi del modello. Il vantaggio di questo approccio è che i dati vengono mantenuti. È possibile apportare questa modifica manualmente o creando uno script di modifica del database.
3. Usare Migrazioni Code First per aggiornare lo schema del database.


Per questa esercitazione si userà Migrazioni Code First.

Aggiornare il metodo di inizializzazione in modo che fornisca un valore per la nuova colonna. Aprire il file Migrations\Configuration.cs e aggiungere un campo di classificazione per ogni oggetto del film.

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

Compilare la soluzione e quindi aprire il **Package Manager Console** finestra e immettere il comando seguente:

`add-migration Rating`

Il `add-migration` comando indica al framework di migrazione per esaminare il modello di film corrente con lo schema di film DB corrente e creare il codice necessario per eseguire la migrazione del database al nuovo modello. Il nome *classificazione* è arbitrario e viene utilizzata per denominare il file di migrazione. È consigliabile utilizzare un nome significativo per il passaggio della migrazione.

Al termine di questo comando, Visual Studio apre il file di classe che definisce la nuova `DbMigration` classe derivata e il `Up` metodo è possibile visualizzare il codice che crea la nuova colonna.

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

Compilare la soluzione e quindi immettere il `update-database` comando il **Package Manager Console** finestra.

La figura seguente mostra l'output nel **Package Manager Console** finestra (il timbro data anteponendo *classificazione* sarà diverso.)

![](adding-a-new-field/_static/image11.png)

Eseguire nuovamente l'applicazione e passare all'URL /Movies. È possibile visualizzare il nuovo campo di classificazione.

![](adding-a-new-field/_static/image12.png)

Fare clic su di **Crea nuovo** collegamento per aggiungere un nuovo film. Si noti che è possibile aggiungere una classificazione.

![7_CreateRioII](adding-a-new-field/_static/image13.png)

Scegliere **Crea**. Il nuovo filmato, tra cui la valutazione, viene inserito nei film elenco:

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

Ora che il progetto utilizza le migrazioni, non è necessario eliminare il database quando si aggiunge un nuovo campo o in caso contrario, aggiornare lo schema. Nella sezione successiva, si sarà apportare ulteriori modifiche allo schema e utilizzare migrazioni per aggiornare il database.

È inoltre necessario aggiungere il `Rating` campo per i modelli di visualizzazione di modifica, eliminazione e dettagli.

È possibile immettere il comando "Aggiorna database" di **Package Manager Console** nuovamente finestra e nessun codice di migrazione potrebbe eseguire l'installazione perché lo schema corrisponde al modello. Tuttavia, in esecuzione "Aggiorna database" verrà eseguito il `Seed` nuovamente, metodo e se è stato modificato, i dati del valore di inizializzazione, le modifiche andranno perse perché il `Seed` dati upserts metodo. Ulteriori informazioni sul `Seed` metodo di Tom Dykstra diffuso [esercitazione ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

In questa sezione è stato illustrato come è possibile modificare gli oggetti del modello e sincronizzare il database con le modifiche. È stato inoltre un modo per popolare un database appena creato con dati di esempio in modo è possibile provare gli scenari. Questo è solo una rapida introduzione alle Code First, vedere [la creazione di un modello di dati di Entity Framework per un'applicazione MVC ASP.NET](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) per un'esercitazione più completa sull'oggetto. Successivamente, si esaminerà come è possibile aggiungere la logica di convalida più completa per le classi di modello e abilitare alcune regole di business da applicare.

> [!div class="step-by-step"]
> [Precedente](adding-search.md)
> [Successivo](adding-validation.md)
