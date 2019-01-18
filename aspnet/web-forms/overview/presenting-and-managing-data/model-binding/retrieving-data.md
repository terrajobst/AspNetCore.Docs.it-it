---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Il recupero e visualizzazione dei dati con modello di associazione e web form | Microsoft Docs
author: Rick-Anderson
description: Questa serie di esercitazioni illustra aspetti di base dell'uso di associazione di modelli con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più linee rette-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: c53c27f4852eab9813bd917315111e7cd3b04953
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396285"
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>Il recupero e visualizzazione dei dati con l'associazione di modelli e web form
====================

> Questa serie di esercitazioni illustra aspetti di base dell'uso di associazione di modelli con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più semplice rispetto a gestione dati di oggetti di origine (ad esempio ObjectDataSource o SqlDataSource). Questa serie inizia con materiale introduttivo e sposta i concetti più avanzati nelle esercitazioni successive.
> 
>  Il modello di associazione del modello funziona con qualsiasi tecnologia di accesso ai dati. In questa esercitazione si userà Entity Framework, ma è possibile usare la tecnologia di accesso ai dati che è più nota all'utente. Da un controllo server associato a dati, ad esempio un controllo GridView, ListView, DetailsView e FormView, si specificano i nomi dei metodi da usare per la selezione, l'aggiornamento, eliminazione e creazione di dati. In questa esercitazione, si specificherà un valore per la proprietà SelectMethod. 
> 
> All'interno di tale metodo, per fornire la logica per recuperare i dati. Nella prossima esercitazione, si imposteranno i valori per UpdateMethod e InsertMethod e DeleteMethod.
>
> È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in C# o Visual Basic. Il codice scaricabile funziona con Visual Studio 2012 e versioni successive. Usa il modello di Visual Studio 2012, che è leggermente diverso rispetto al modello di Visual Studio 2017 illustrato in questa esercitazione.
> 
> In questa esercitazione si esegue l'applicazione in Visual Studio. È anche possibile distribuire l'applicazione in un provider di hosting e renderlo disponibile tramite internet. Microsoft offre l'hosting web gratuito per fino a 10 siti web in un  
>  [account Azure gratuito](https://azure.microsoft.com/free/?WT.mc_id=A443DD604). Per informazioni su come distribuire un progetto web Visual Studio per App Web di servizio App di Azure, vedere la [distribuzione Web ASP.NET tramite Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) serie. Tale esercitazione illustra anche come usare le migrazioni di Entity Framework Code First per distribuire il database di SQL Server in Database SQL di Azure.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> - Microsoft Visual Studio 2017 or Microsoft Visual Studio Community 2017
>   
> Questa esercitazione funziona anche con Visual Studio 2012 e Visual Studio 2013, ma esistono alcune differenze nel modello di progetto e dell'interfaccia utente.


## <a name="what-youll-build"></a>Scopo dell'esercitazione

In questa esercitazione, sarà:

* Creare oggetti dati che riflettono un'università con studenti iscritti a corsi
* Creare tabelle di database dagli oggetti
* Popolare il database con dati di test
* Visualizzare i dati in un web form

## <a name="create-the-project"></a>Creare il progetto

1. In Visual Studio 2017, creare un **applicazione Web ASP.NET (.NET Framework)** progetto chiamato **ContosoUniversityModelBinding**.

   ![Crea progetto](retrieving-data/_static/image19.png)

2. Scegliere **OK**. Viene visualizzata la finestra di dialogo per selezionare un modello.

   ![Selezionare web form](retrieving-data/_static/image3.png)

3. Selezionare il **Web Form** modello. 

4. Se necessario, modificare l'autenticazione **singoli account utente di**. 

5. Selezionare **OK** per creare il progetto.

## <a name="modify-site-appearance"></a>Modificare l'aspetto del sito

   Apportare alcune modifiche per personalizzare l'aspetto del sito. 
   
   1. Aprire il file Site. master.
   
   2. Modificare il titolo da visualizzare **Contoso University** e non **My ASP.NET Application**.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

   3. Modificare il testo dell'intestazione dal **nome dell'applicazione** al **Contoso University**.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

   4. Modificare i collegamenti di intestazione di navigazione per le colonne appropriate del sito. 
   
      Rimuovere i collegamenti per **sulle** e **Contact** e, invece, crea un collegamento a una **studenti** pagina, che verrà creato.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample3.aspx)]

   5. Salvare Site. master.

## <a name="add-a-web-form-to-display-student-data"></a>Aggiungere un web form per visualizzare i dati degli studenti

   1. Nelle **Esplora soluzioni**, fare clic sul progetto, selezionare **Add** e quindi **nuovo elemento**. 
   
   2. Nel **Aggiungi nuovo elemento** finestra di dialogo, seleziona la **Web Form con pagina Master** modello e denominarlo **Students.aspx**.

      ![pagina Crea](retrieving-data/_static/image5.png)

   3. Selezionare **Aggiungi**.
   
   4. Per la pagina master del web form, selezionare **Site. master**.
   
   5. Scegliere **OK**.
   

## <a name="add-the-data-model"></a>Aggiungere il modello di dati

Nel **modelli** cartella, aggiungere una classe denominata **UniversityModels.cs**.

   1. Fare doppio clic su **modelli**, selezionare **Add**e quindi **nuovo elemento**. Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.

   2. Nel menu di spostamento a sinistra, selezionare **codice**, quindi **classe**.

      ![creare una classe di modello](retrieving-data/_static/image20.png)

   3. Denominare la classe **UniversityModels.cs** e selezionare **Add**.

      In questo file, definire le `SchoolContext`, `Student`, `Enrollment`, e `Course` classi come indicato di seguito:

      [!code-csharp[Main](retrieving-data/samples/sample4.cs)]

      Il `SchoolContext` deriva dalla classe `DbContext`, che gestisce la connessione al database e modifica dei dati.

      Nel `Student` classe, si noti che gli attributi applicati al `FirstName`, `LastName`, e `Year` proprietà. Questa esercitazione Usa questi attributi per la convalida dei dati. Per semplificare il codice, queste proprietà sono contrassegnate con attributi di convalida dei dati. In un progetto reale, gli attributi di convalida viene applicato a tutte le proprietà che necessitano di convalida.

   4. Save UniversityModels.cs.

## <a name="set-up-the-database-based-on-classes"></a>Configurare il database basato su classi

Questa esercitazione viene usato [migrazioni Code First](https://docs.microsoft.com/en-us/ef/ef6/modeling/code-first/migrations/) per creare oggetti e tabelle di database. Queste tabelle archiviano informazioni agli studenti e i rispettivi corsi.

   1. Selezionare **strumenti di** > **Gestione pacchetti NuGet** > **la Console di Gestione pacchetti**.

   2. Nelle **Console di gestione pacchetti**, eseguire questo comando:  
      `enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

      Se il comando viene completato correttamente, viene visualizzato un messaggio che indica che le migrazioni sono state abilitate.

      ![abilitare migrazioni](retrieving-data/_static/image8.png)

      Si noti che un file denominato *Configuration.cs* è stato creato. Il `Configuration` classe ha un `Seed` metodo, che è possibile popolare anticipatamente tabelle del database con dati di test.

## <a name="pre-populate-the-database"></a>Pre-popolare il database

   1. Aprire Configuration.cs.
   
   2. Aggiungere al metodo `Seed` il seguente codice. Aggiungere anche un `using` istruzione per il `ContosoUniversityModelBinding. Models` dello spazio dei nomi.

      [!code-csharp[Main](retrieving-data/samples/sample5.cs)]

   3. Salvare Configuration.cs.

   4. Nella Console di gestione pacchetti eseguire il comando **aggiungere la migrazione iniziale**.

   5. Eseguire il comando **update-database**.

      Se si riceve un'eccezione quando si esegue questo comando, il `StudentID` e `CourseID` valori potrebbero essere diversi dal `Seed` i valori del metodo. Aprire le tabelle di database e i valori esistenti per trovare `StudentID` e `CourseID`. Aggiungere tali valori per il codice per il seeding di `Enrollments` tabella.

## <a name="add-a-gridview-control"></a>Aggiungere un controllo GridView

Con dati popolati del database, ora possibile recuperare tali dati e visualizzarli. 

1. Aprire Students.aspx.

2. Individuare il `MainContent` segnaposto. All'interno di tale segnaposto, aggiungere un **GridView** controllo che include il codice.

   [!code-aspx-csharp[Main](retrieving-data/samples/sample6.aspx)]

   Note importanti:
   * Si noti che il valore impostato per il `SelectMethod` proprietà nell'elemento GridView. Questo valore specifica il metodo utilizzato per recuperare i dati di GridView, che creato nel passaggio successivo. 
   
   * Il `ItemType` è impostata sul `Student` classe creata in precedenza. Questa impostazione consente di fare riferimento alle proprietà di classe nel markup. Ad esempio, il `Student` classe dispone di una raccolta denominata `Enrollments`. È possibile usare `Item.Enrollments` per recuperare tale insieme e quindi usare [sintassi LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) per recuperare ogni studente del registrati somma i crediti.
   
3. Salvare Students.aspx.

## <a name="add-code-to-retrieve-data"></a>Aggiungere il codice per recuperare i dati

   Nel file code-behind Students.aspx, aggiungere il metodo specificato per il `SelectMethod` valore. 
   
   1. Aprire Students.aspx.cs.
   
   2. Aggiungere `using` istruzioni per il `ContosoUniversityModelBinding. Models` e `System.Data.Entity` gli spazi dei nomi.

      [!code-csharp[Main](retrieving-data/samples/sample7.cs)]

   3. Aggiungere il metodo specificato per `SelectMethod`:

      [!code-csharp[Main](retrieving-data/samples/sample8.cs)]

      Il `Include` clausola consente di migliorare le prestazioni delle query ma non è obbligatorio. Senza il `Include` clausola, i dati verrà recuperato utilizzando [ *caricamento lazy*](https://en.wikipedia.org/wiki/Lazy_loading), che prevede l'invio di una query separata per il database ogni volta che correlati vengono recuperati i dati. Con il `Include` clausola, i dati verrà recuperato utilizzando *caricamento eager*, ovvero un'unica query al database recupera tutti i dati correlati. Se i dati correlati non viene usati, il caricamento eager è meno efficiente perché vengono recuperati più dati. Tuttavia, in questo caso, il caricamento eager offre prestazioni ottimali perché i dati correlati vengono visualizzati per ogni record.

      Per altre informazioni sulle considerazioni sulle prestazioni quando si caricano i dati correlati, vedere la **esplicito il caricamento di dati correlati Lazy ed Eager** sezione il [la lettura di dati correlati con Entity Framework in ASP.NET Applicazione MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) articolo.

      Per impostazione predefinita, i dati vengono ordinati i valori della proprietà contrassegnata come chiave. È possibile aggiungere un `OrderBy` clausola per specificare un valore di ordinamento diversi. In questo esempio, il valore predefinito `StudentID` proprietà viene utilizzata per l'ordinamento. Nel [ordinamento, Paging e filtro dei dati](sorting-paging-and-filtering-data.md) articolo, l'utente è abilitato per selezionare una colonna per l'ordinamento.
 
   4. Salvare Students.aspx.cs.

## <a name="run-your-application"></a>Eseguire l'applicazione 

Eseguire l'applicazione web (**F5**) e passare alle **studenti** pagina, che verrà visualizzato quanto segue:

   ![visualizzare i dati](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>Generazione automatica di metodi di associazione di modelli

Quando questa serie di esercitazioni, è possibile copiare semplicemente il codice dell'esercitazione al progetto. Tuttavia, uno svantaggio di questo approccio è che si potrebbe non viene a conoscenza della funzionalità fornite da Visual Studio per generare automaticamente codice per metodi di associazione di modelli. Quando si lavora su progetti, generazione automatica di codice può risparmiare tempo e la Guida è possibile ottenere un'idea di come implementare un'operazione. In questa sezione viene descritta la funzionalità di generazione automatica di codice. Questa sezione è solo informativo e non contiene alcun codice che è necessario implementare nel progetto. 

Quando si imposta un valore per il `SelectMethod`, `UpdateMethod`, `InsertMethod`, o `DeleteMethod` le proprietà nel codice di markup, è possibile selezionare il **creare nuovo metodo** opzione.

![creare un metodo](retrieving-data/_static/image18.png)

Visual Studio non solo crea un metodo nel code-behind con la firma appropriata, ma genera anche codice di implementazione per eseguire l'operazione. Se si imposta innanzitutto il `ItemType` proprietà prima di usare la generazione automatica di codice delle funzionalità, il codice generato viene utilizzato dai tipi per le operazioni. Ad esempio, quando si impostano le `UpdateMethod` proprietà, il codice seguente viene generato automaticamente:

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

Anche in questo caso, questo codice non deve essere aggiunto al progetto. Nella prossima esercitazione, verrà implementato metodi per l'aggiornamento, eliminazione e aggiunta di nuovi dati.

## <a name="summary"></a>Riepilogo

In questa esercitazione si create classi di modello di dati e generata un database da tali classi. Le tabelle del database è stato compilato con i dati di test. È utilizzata l'associazione di modelli per recuperare dati dal database e quindi visualizzati i dati in un controllo GridView.

Entro i prossimi [esercitazione](updating-deleting-and-creating-data.md) in questa serie, verrà abilitata l'aggiornamento, eliminazione e creazione di dati.

> [!div class="step-by-step"]
> [avanti](updating-deleting-and-creating-data.md)
