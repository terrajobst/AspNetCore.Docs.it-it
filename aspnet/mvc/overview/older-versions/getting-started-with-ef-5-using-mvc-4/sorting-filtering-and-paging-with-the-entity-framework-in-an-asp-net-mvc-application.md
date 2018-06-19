---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: Ordinamento, filtro e Paging con Entity Framework in un'applicazione ASP.NET MVC (3 di 10) | Documenti Microsoft
author: tdykstra
description: L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 8af630e0-fffa-4110-9eca-c96e201b2724
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 09327b760d9be38d7e004cbcef08cad4eab3a26c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878230"
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application-3-of-10"></a>Ordinamento, filtro e Paging con Entity Framework in un'applicazione ASP.NET MVC (3 di 10)
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 4 utilizzando Code First di Entity Framework 5 e Visual Studio 2012. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). È possibile avviare la serie di esercitazioni dall'inizio o [scaricare un progetto di avvio per questo capitolo](building-the-ef5-mvc4-chapter-downloads.md) e iniziare da qui.
> 
> > [!NOTE] 
> > 
> > Se si esegue un problema, è possibile risolvere, [scaricare il capitolo completato](building-the-ef5-mvc4-chapter-downloads.md) e provare a riprodurre il problema. In genere, è possibile trovare la soluzione al problema di mediante un confronto tra il codice per il codice completato. Per alcuni errori comuni e su come risolverli, vedere [errori e soluzioni alternative.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Nell'esercitazione precedente è stato implementato un set di pagine web per operazioni CRUD di base per `Student` entità. In questa esercitazione si aggiungerà ordinamento, filtro e la funzionalità di paging per il **studenti** pagina di indice. Verrà anche creata una pagina che esegue il raggruppamento semplice.

La figura seguente illustra l'aspetto della pagina al termine dell'operazione. Le intestazioni di colonna sono collegamenti su cui l'utente può fare clic per eseguire l'ordinamento in base alla colonna. Facendo clic più volte su un'intestazione di colonna è possibile passare dall'ordinamento crescente a quello decrescente e viceversa.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Aggiungere collegamenti per l'ordinamento di colonna alla pagina Student Index (Indice degli studenti)

Per aggiungere l'ordinamento alla pagina di indice di studenti, sarà necessario impostare il `Index` metodo il `Student` controller e aggiungere codice per il `Student` indicizzare la vista.

### <a name="add-sorting-functionality-to-the-index-method"></a>Aggiungere funzionalità al metodo di indice di ordinamento

In *Controllers\StudentController.cs*, sostituire il `Index` (metodo) con il codice seguente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Questo codice riceve un parametro `sortOrder` dalla stringa di query nell'URL. Il valore di stringa di query viene fornito da ASP.NET MVC come parametro al metodo di azione. Il parametro sarà una stringa "Name" o "Date", seguito facoltativamente da un carattere di sottolineatura e dalla stringa "desc" per specificare l'ordine decrescente. L'ordinamento predefinito è crescente.

La prima volta che viene richiesta la pagina di indice, non è presente alcuna stringa di query. Gli studenti vengono visualizzati in ordine crescente in base `LastName`, ovvero il valore predefinito definito dalla istruzione case nel `switch` istruzione. Quando l'utente fa clic sul collegamento ipertestuale di un'intestazione di colonna, nella stringa di query viene specificato il valore `sortOrder` appropriato.

I due `ViewBag` variabili vengono utilizzate in modo che la vista è possibile configurare i collegamenti ipertestuali sull'intestazione di colonna con i valori di stringa di query appropriata:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Si tratta di istruzioni ternarie. Il primo specifica che se il `sortOrder` parametro è null o vuoto, `ViewBag.NameSortParm` deve essere impostato su "nome\_desc"; in caso contrario, deve essere impostato su una stringa vuota. Queste due istruzioni consentono alla visualizzazione di impostare i collegamenti ipertestuali dell'intestazione di colonna come indicato di seguito:

| Ordinamento corrente | Collegamento ipertestuale cognome | Collegamento ipertestuale data |
| --- | --- | --- |
| Cognome in ordine crescente | descending | ascending |
| Cognome in ordine decrescente | ascending | ascending |
| Data in ordine crescente | ascending | descending |
| Data in ordine decrescente | ascending | ascending |

Il metodo utilizza [LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx) per specificare la colonna da ordinare. Il codice crea un [IQueryable](https://msdn.microsoft.com/library/bb351562.aspx) variabile prima il `switch` istruzione modifica nel `switch` istruzione e chiama il `ToList` metodo dopo il `switch` istruzione. Quando si creano e modificano variabili `IQueryable`, nessuna query viene inviata al database. La query non viene eseguita fino alla conversione di `IQueryable` oggetto in una raccolta chiamando un metodo, ad esempio `ToList`. Pertanto, questo codice genera una singola query che non viene eseguita finché il `return View` istruzione.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Aggiungere collegamenti ipertestuali per la visualizzazione dell'indice per studenti di intestazione di colonna

In *Views\Student\Index.cshtml*, sostituire il `<tr>` e `<th>` elementi per la riga di intestazione con il codice evidenziato di seguito:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

Questo codice Usa le informazioni di `ViewBag` valori di stringa di proprietà per impostare i collegamenti ipertestuali con la query appropriato.

Eseguire la pagina e fare clic su di **cognome** e **data di registrazione** funziona intestazioni di colonna per verificare che l'ordinamento.

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Dopo aver selezionato il **Last Name** titolo, gli studenti vengono visualizzati in ultimo nome ordine decrescente.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Aggiungere una casella di ricerca per la pagina di indice di studenti

Per aggiungere filtri alla pagina Student Index (Indice degli studenti), è necessario aggiungere alla visualizzazione una casella di testo e un pulsante di invio e apportare le modifiche corrispondenti nel metodo `Index`. La casella di testo consente di immettere una stringa per eseguire la ricerca nei campi di nome e cognome.

### <a name="add-filtering-functionality-to-the-index-method"></a>Aggiungere funzionalità di filtro per il metodo di Index

In *Controllers\StudentController.cs*, sostituire il `Index` (metodo) con il codice seguente (le modifiche vengono evidenziate):

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

È stato aggiunto un parametro `searchString` al metodo `Index`. Anche aggiunte all'istruzione LINQ un `where` clausethat seleziona solo gli studenti il cui nome o cognome contengono la stringa di ricerca. Il valore di stringa di ricerca viene ricevuto da una casella di testo che verrà aggiunto per la visualizzazione dell'indice. L'istruzione che aggiunge il [dove](https://msdn.microsoft.com/library/bb535040.aspx) clausola viene eseguita solo se è presente un valore per la ricerca.

> [!NOTE]
> In molti casi è possibile chiamare lo stesso metodo su un set di entità di Entity Framework o come metodo di estensione per una raccolta in memoria. I risultati sono in genere uguali, ma in alcuni casi potrebbero essere diversi. Ad esempio, l'implementazione di .NET Framework del `Contains` restituisce tutte le righe quando è passare una stringa vuota, ma il provider di Entity Framework per SQL Server Compact 4.0 restituisce zero righe, le stringhe vuote. Pertanto il codice di esempio (inserisce il `Where` istruzione all'interno di un `if` istruzione) rende è necessario ottenere gli stessi risultati per tutte le versioni di SQL Server. Inoltre, l'implementazione di .NET Framework del `Contains` metodo esegue un confronto tra maiuscole e minuscole per impostazione predefinita, ma per impostazione predefinita, i provider di Entity Framework SQL Server eseguono confronti tra maiuscole e minuscole. Pertanto, la chiamata di `ToUpper` metodo per eseguire il test in modo esplicito con distinzione tra maiuscole e assicura che non cambiano quando si modifica il codice in un secondo momento per utilizzare un repository, che restituirà un `IEnumerable` raccolta invece di un `IQueryable` oggetto. Quando il metodo `Contains` viene chiamato su una raccolta `IEnumerable`, si ottiene l'implementazione di .NET Framework; quando viene chiamato su un oggetto `IQueryable`, si ottiene l'implementazione del provider di database.


### <a name="add-a-search-box-to-the-student-index-view"></a>Aggiungere una casella di ricerca alla visualizzazione Student Index (Indice degli studenti)

In *Views\Student\Index.cshtml*, aggiungere il codice evidenziato immediatamente prima dell'apertura `table` tag per creare una didascalia, una casella di testo e un **ricerca** pulsante.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Eseguire la pagina, immettere una stringa di ricerca e fare clic su **ricerca** per verificare che il filtro sia funzionante.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Si noti che l'URL non contiene "an" stringa di ricerca, il che significa che se questa pagina, non si otterrà l'elenco filtrato quando si utilizza il segnalibro. Si modificherà il **ricerca** pulsante da usare le stringhe di query per i criteri di filtro più avanti nell'esercitazione.

## <a name="add-paging-to-the-students-index-page"></a>Aggiungere il Paging per la pagina di indice di studenti

Per aggiungere il paging per la pagina di indice di studenti, si inizierà installando il **PagedList.Mvc** pacchetto NuGet. Quindi è possibile apportare ulteriori modifiche nel `Index` (metodo) e aggiungere collegamenti di paging per il `Index` visualizzazione. **PagedList.Mvc** è uno dei molti paging ottimale e l'ordinamento dei pacchetti per ASP.NET MVC, ed è destinato solo l'uso di seguito un esempio, non come un suggerimento con altre opzioni. Nella figura seguente vengono visualizzati i collegamenti di spostamento.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>Installare il pacchetto PagedList.MVC NuGet

NuGet **PagedList.Mvc** pacchetto installa automaticamente il **PagedList** pacchetto come dipendenza. Il **PagedList** pacchetto installa un `PagedList` metodi di estensione e di tipo di raccolta per `IQueryable` e `IEnumerable` raccolte. I metodi di estensione per creano una singola pagina di dati in un `PagedList` raccolta fuori il `IQueryable` o `IEnumerable`e `PagedList` raccolta fornisce molte proprietà e metodi che facilitano lo spostamento. Il **PagedList.Mvc** pacchetto viene installato un supporto di paging che visualizza i pulsanti di spostamento.

Dal **strumenti** dal menu **Gestione pacchetti libreria** e quindi **Gestisci pacchetti NuGet per la soluzione**.

Nel **Gestisci pacchetti NuGet** la finestra di dialogo, fare clic su di **Online** scheda a sinistra e quindi immettere "del pool di paging" nella casella di ricerca. Quando viene visualizzato il **PagedList.Mvc** del pacchetto, fare clic su **installare**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Nel **selezionare progetti** fare clic su **OK**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="add-paging-functionality-to-the-index-method"></a>Aggiungere funzionalità di Paging per il metodo di Index

In *Controllers\StudentController.cs*, aggiungere un `using` istruzione per il `PagedList` dello spazio dei nomi:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Sostituire il metodo `Index` con il codice seguente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Questo codice aggiunge un `page` parametro, un parametro di ordine di ordinamento corrente e un parametro di filtro corrente per la firma del metodo, come illustrato di seguito:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

La prima volta che viene visualizzata la pagina o se l'utente non ha selezionato un collegamento di suddivisione in pagine o di ordinamento, tutti i parametri saranno Null. Se si seleziona un collegamento di paging, il `page` variabile conterrà il numero di pagina da visualizzare.

`A ViewBag` proprietà fornisce la vista con l'ordinamento corrente, in quanto ciò deve essere inclusi i collegamenti di spostamento per mantenere lo stesso durante il paging dell'ordinamento:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Un'altra proprietà, `ViewBag.CurrentFilter`, fornisce la visualizzazione con la stringa di filtro corrente. Questo valore deve essere incluso nei collegamenti di suddivisione in pagine per mantenere le impostazioni di filtro nella suddivisione in pagine e deve essere ripristinato nella casella di testo quando la pagina viene nuovamente visualizzata. Se la stringa di ricerca viene modificata nella suddivisione in pagine, la pagina deve essere reimpostata su 1, poiché il nuovo filtro può comportare la visualizzazione di dati diversi. La stringa di ricerca viene modificata quando viene immesso un valore nella casella di testo e viene premuto il pulsante Invia. In tal caso, il `searchString` parametro non è null.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Alla fine del metodo, il `ToPagedList` metodo di estensione su studenti `IQueryable` oggetto converte la query di studenti in una singola pagina degli studenti iscritti a un tipo di insieme che supporta il paging. Pagina singola di studenti viene quindi passato alla visualizzazione:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Il metodo `ToPagedList` accetta un numero di pagina. Due punti interrogativi rappresentano il [operatore null-coalescing](https://msdn.microsoft.com/library/ms173224.aspx). L'operatore null-coalescing definisce un valore predefinito per un tipo nullable. L'espressione `(page ?? 1)` significa restituzione del valore di `page` se ha un valore oppure restituzione di 1 se `page` è Null.

### <a name="add-paging-links-to-the-student-index-view"></a>Aggiungere collegamenti di spostamento per la visualizzazione dell'indice per studenti

In *Views\Student\Index.cshtml*, sostituire il codice esistente con il codice seguente:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=6,9,14-20,56-58)]

L'istruzione `@model` nella parte superiore della pagina specifica che la vista ottiene ora un oggetto `PagedList` anziché un oggetto `List`.

Il `using` istruzione per `PagedList.Mvc` consente di accedere al supporto MVC per i pulsanti di spostamento.

Il codice utilizza un overload di [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) che consente di specificare [FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

Il valore predefinito [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) invia i dati del form con un POST, il che significa che i parametri vengono passati nel corpo del messaggio HTTP e non nell'URL come stringhe di query. Quando si specifica HTTP GET, i dati del modulo vengono passati nell'URL come stringhe di query, il che consente agli utenti di inserire l'URL tra i segnalibri. Il [W3C linee guida per l'utilizzo di HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) specificare che, quando l'azione non comporta un aggiornamento, è necessario utilizzare GET.

La casella di testo viene inizializzata con la stringa di ricerca corrente quando si fa clic su una nuova pagina per visualizzare la stringa di ricerca corrente.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

I collegamenti delle intestazioni di colonna usano la stringa di query per passare la stringa di ricerca corrente al controller in modo che l'utente possa procedere all'ordinamento all'interno dei risultati di filtro:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

Viene visualizzato il numero corrente di pagina e il totale di pagine.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

Se non sono presenti pagine per la visualizzazione, viene visualizzata "Pagina 0 0". (In tal caso il numero di pagina è maggiore del numero di pagina perché `Model.PageNumber` è 1, e `Model.PageCount` è 0.)

Vengono visualizzati i pulsanti di spostamento per la `PagedListPager` helper:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

Il `PagedListPager` helper fornisce una serie di opzioni che è possibile personalizzare, inclusi URL e lo stile. Per ulteriori informazioni, vedere [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) sul sito GitHub.

Eseguire la pagina.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Fare clic sui collegamenti di suddivisione in pagine in diversi tipi di ordinamento per verificare che la suddivisione in pagine funzioni. Immettere quindi una stringa di ricerca e provare nuovamente la suddivisione in pagine per verificare che funzioni correttamente anche con l'ordinamento e il filtro.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Creare una pagina che mostra le statistiche di Student

Per Contoso University del sito Web sulla pagina, verranno visualizzati il numero di studenti sono registrati per ciascuna data di registrazione. Questa operazione richiede calcoli di raggruppamento e semplici sui gruppi. Per completare questa procedura, è necessario eseguire le operazioni seguenti:

- Creare una classe modello di visualizzazione per i dati che è necessario passare alla visualizzazione.
- Modificare il `About` metodo il `Home` controller.
- Modificare il `About` visualizzazione.

### <a name="create-the-view-model"></a>Creare il modello di visualizzazione

Creare un *ViewModel* cartella. In tale cartella, aggiungere un file di classe *EnrollmentDateGroup.cs* e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Modificare il controller Home

In *HomeController.cs*, aggiungere il seguente `using` istruzioni nella parte superiore del file:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Aggiungere una variabile di classe per il contesto del database immediatamente dopo la parentesi graffa di apertura per la classe:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

Sostituire il metodo `About` con il codice seguente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

L'istruzione LINQ raggruppa le entità di studenti per data di registrazione, calcola il numero di entità in ogni gruppo e archivia i risultati in una raccolta di oggetti di modello della visualizzazione `EnrollmentDateGroup`.

Aggiungere un `Dispose` metodo:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Modificare la visualizzazione della pagina About (Informazioni)

Sostituire il codice di *Views\Home\About.cshtml* file con il codice seguente:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

Eseguire l'app e fare clic su di **su** collegamento. Il numero di studenti per ogni data di registrazione viene visualizzato in una tabella.

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

## <a name="optional-deploy-the-app-to-windows-azure"></a>Facoltativo: Distribuire l'app in Windows Azure

Finora l'applicazione è stato in esecuzione in locale in IIS Express nel computer di sviluppo. Per renderlo disponibile ad altri utenti per l'utilizzo su Internet, è necessario distribuirla in un provider di hosting web. In questa sezione facoltativa dell'esercitazione è possibile distribuirlo a un sito Web di Windows Azure.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Utilizzare migrazioni Code First per distribuire il Database

Per distribuire il database utilizzare migrazioni Code First. Quando si crea il profilo di pubblicazione che consente di configurare le impostazioni per la distribuzione da Visual Studio, si selezionano una casella di controllo etichettato **eseguire migrazioni Code First (esecuzione all'avvio dell'applicazione)**. Questa impostazione, il processo di distribuzione configurare automaticamente l'applicazione *Web. config* file nel server di destinazione in modo che usa Code First di `MigrateDatabaseToLatestVersion` classe inizializzatore.

Visual Studio non esegue alcuna operazione con il database durante il processo di distribuzione. Quando l'applicazione distribuita accede al database per la prima volta dopo la distribuzione, Code First automaticamente viene creato il database o aggiorna lo schema del database alla versione più recente. Se l'applicazione implementa un migrazioni `Seed` metodo, l'esecuzione del metodo dopo la creazione del database o lo schema viene aggiornato.

Le migrazioni `Seed` metodo inserisce i dati di test. Se sono stati distribuito in un ambiente di produzione, è necessario modificare il `Seed` metodo in modo che venga inserito solo dati che si desidera essere inseriti nel database di produzione. Ad esempio, nel modello di dati corrente è potrebbe desidera disporre corsi reali ma studenti fittizi nel database di sviluppo. È possibile scrivere un `Seed` metodo per caricare entrambe in fase di sviluppo e quindi impostare come commento studenti fittizi prima di distribuire nell'ambiente di produzione. In alternativa è possibile scrivere un `Seed` metodo per caricare solo i corsi e immettere gli studenti fittizi manualmente nel database di prova tramite interfaccia utente dell'applicazione.

### <a name="get-a-windows-azure-account"></a>Ottenere un account di Windows Azure

È necessario un account di Windows Azure. Se non hai già uno, è possibile creare un account di valutazione gratuito in pochi minuti. Per informazioni dettagliate, vedere [versione di valutazione gratuita di Windows Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

### <a name="create-a-web-site-and-a-sql-database-in-windows-azure"></a>Creare un sito web e un database SQL in Windows Azure

Il sito Web di Microsoft Azure verrà eseguito in un ambiente di hosting condiviso, ovvero che viene eseguito su macchine virtuali (VM) che vengono condivisi con altri client di Windows Azure. Un ambiente di hosting condiviso è una soluzione a basso costo per iniziare nel cloud. In un secondo momento, se il traffico web aumenta, l'applicazione scalabile per soddisfare le esigenze di mediante l'esecuzione su macchine virtuali dedicate. Se è necessaria un'architettura più complessa, è possibile eseguire la migrazione a un servizio Cloud di Windows Azure. Servizi cloud vengono eseguiti in macchine virtuali dedicate che è possibile configurare in base alle esigenze.

Database SQL di Azure è un servizio di database relazionale basato su cloud sviluppato su tecnologie di SQL Server. Strumenti e applicazioni che funzionano con SQL Server funzionano anche con il Database SQL.

1. Nel [il portale di gestione di Windows Azure](https://manage.windowsazure.com/), fare clic su **siti Web** nel riquadro a sinistra e quindi fare clic su **New**.

    ![Pulsante nuovo nel portale di gestione](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)
2. Fare clic su **creazione personalizzata**.

    ![Creare con il collegamento di Database nel portale di gestione](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

   Il **nuovo sito Web - creazione personalizzata** apre la procedura guidata.
3. Nel **nuovo sito Web** passaggio della procedura guidata, immettere una stringa di **URL** casella da usare come URL univoco per l'applicazione. L'URL completo sarà costituito da quelle immesse, più il suffisso che viene visualizzato accanto alla casella di testo. Nell'illustrazione "ConU", ma tale URL è probabilmente ricavato pertanto è necessario sceglierne uno diverso.

    ![Creare con il collegamento di Database nel portale di gestione](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)
4. Nel **area** elenco a discesa scegliere una regione vicina è. Questa impostazione specifica quale data center in cui verrà eseguito il sito web.
5. Nel **Database** elenco a discesa scegliere **crea un database SQL 20 MB gratuito**.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)
6. Nel **DB CONNECTION STRING NAME**, immettere *SchoolContext*.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)
7. Fare clic sulla freccia rivolta verso destra nella parte inferiore della finestra. Verrà visualizzata la **le impostazioni del Database** passaggio.
8. Nel **nome** immettere *ContosoUniversityDB*.
9. Nel **Server** , quindi selezionare **server nuovo Database SQL**. In alternativa, se un server è stato creato in precedenza, è possibile selezionare server nell'elenco a discesa.
10. Immettere un amministratore **nome account di accesso** e **PASSWORD**. Se si seleziona **server nuovo Database SQL** non immettere un nome esistente e la password in questo caso, si sta immettendo un nuovo nome e una password che si sta definendo ora da utilizzare in un secondo momento quando si accede al database. Se si seleziona un server in cui è stato creato in precedenza, si immetteranno le credenziali per il server. Per questa esercitazione, non selezionare la ***avanzate*** casella di controllo. Il ***avanzate*** opzioni consentono di impostare il database [delle regole di confronto](https://msdn.microsoft.com/library/aa174903(v=SQL.80).aspx).
11. Scegliere lo stesso **area** scelto per il sito web.
12. Fare clic sul segno di spunta nella parte inferiore destra della casella per indicare che si è finito.   
  
    ![Passaggio di impostazioni di database del sito Web - creare con Creazione guidata Database](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image16.png)  

    Nella figura seguente viene illustrato l'utilizzo di un Server SQL esistente e un account di accesso.   
  
    ![Passaggio di impostazioni di database del sito Web - creare con Creazione guidata Database](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image17.png)  
  
    Il portale di gestione restituisce alla pagina di siti Web e **stato** colonna mostra che il sito viene creato. Dopo un periodo di tempo (in genere inferiore a un minuto), il **stato** colonna mostra che il sito è stato creato correttamente. Nella barra di spostamento a sinistra, il numero di siti presenti nel tuo account viene visualizzato accanto al **siti Web** icona e il numero di database viene visualizzato accanto al **database SQL** icona.

## <a name="deploy-the-application-to-windows-azure"></a>Distribuire l'applicazione in Windows Azure

1. In Visual Studio, fare clic sul progetto in **Esplora** e selezionare **pubblica** dal menu di scelta rapida.  
  
    ![Pubblicare nel menu di scelta rapida progetto](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image18.png)
2. Nel **profilo** scheda della finestra di **pubblica sul Web** procedura guidata, fare clic su **importazione**.  
  
    ![Importazione delle impostazioni di pubblicazione.](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image19.png)
3. Se non è stato aggiunto in precedenza la sottoscrizione Windows Azure in Visual Studio, eseguire la procedura seguente. In questa procedura si aggiunta la sottoscrizione in modo che l'elenco a discesa dell'opzione **importazione da un sito web di Windows Azure** includerà il sito web.

    a. Nel **Importa profilo di pubblicazione** la finestra di dialogo, fare clic su **importazione da un sito web di Windows Azure**e quindi fare clic su **sottoscrizione di Azure aggiungere**.

    ![Aggiungi sottoscrizione Windows Azure](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image20.png)

    b. Nel **Importa sottoscrizioni di Windows Azure** la finestra di dialogo, fare clic su **Download file di sottoscrizione**.

    ![scaricare i file di sottoscrizione](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image21.png)

    c. Nella finestra del browser, salvare il *publishsettings* file.

    ![scaricare i file con estensione publishsettings](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image22.png)

    > [!WARNING]
    > Sicurezza: il *publishsettings* file contiene le credenziali (non codificate) utilizzate per amministrare i servizi e le sottoscrizioni Windows Azure. La procedura consigliata protezione per questo file consiste nell'archiviarlo temporaneamente all'esterno delle directory di origine (ad esempio nel *raccolte\documenti* cartella) e quindi eliminarlo dopo l'importazione è stata completata. Un utente malintenzionato che accede al `.publishsettings` file può modificare, creare ed eliminare i servizi di Windows Azure.

    d. Nel **Importa sottoscrizioni di Windows Azure** la finestra di dialogo, fare clic su **Sfoglia** e passare al *publishsettings* file.

    ![scaricare sub](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image23.png)

    e. Fare clic su **Importa**.

    ![importazione](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image24.png)
4. Nel **Importa profilo di pubblicazione** nella finestra di dialogo **importazione da un sito web di Windows Azure**, selezionare il sito web dall'elenco a discesa e quindi fare clic su **OK**.  
  
    ![Importazione del profilo di pubblicazione](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image25.png)
5. Nel **connessione** scheda, fare clic su **convalida connessione** per assicurarsi che le impostazioni siano corrette.  
  
    ![Convalidare la connessione](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)
6. Quando la connessione è stata convalidata, un segno di spunta verde viene visualizzato accanto al **convalida connessione** pulsante. Scegliere **Avanti**.  
  
    ![Connessione convalidata correttamente](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image27.png)
7. Aprire il **stringa di connessione remota** elenco a discesa in **SchoolContext** e selezionare la stringa di connessione per il database è stato creato.
8. Selezionare **eseguire migrazioni Code First (esecuzione all'avvio dell'applicazione)**.
9. Deselezionare **utilizza stringa di connessione in fase di esecuzione** per il **UserContext (DefaultConnection)**, dal momento che l'applicazione non utilizza il database delle appartenenze.   
  
    ![Scheda Impostazioni](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image28.png)
10. Scegliere **Avanti**.
11. Nel **anteprima** scheda, fare clic su **avviare Anteprima**.  
  
    ![Pulsante StartPreview nella scheda Anteprima](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image29.png)  
  
    La scheda Visualizza un elenco di file che verranno copiati nel server. La visualizzazione dell'anteprima non è necessario pubblicare l'applicazione, ma è una funzione utile conoscere. In questo caso, non occorre eseguire alcuna operazione con l'elenco di file che viene visualizzato. Alla successiva che si distribuisce questa applicazione, sarà solo i file che sono stati modificati in questo elenco.  
  
    ![Output del file StartPreview](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image30.png)
12. Fare clic su **Pubblica**.  
    Visual Studio avvia il processo di copia dei file per il server Windows Azure.
13. Il **Output** finestra Mostra intraprese le azioni di distribuzione e segnala il completamento corretto della distribuzione.  
  
    ![Finestra di output corretta distribuzione di reporting](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image31.png)
14. Durante la distribuzione ha esito positivo, il browser predefinito verrà aperta automaticamente per l'URL del sito web distribuito.  
    L'applicazione creata è in esecuzione nel cloud. Fare clic sulla scheda studenti.  
  
    ![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image32.png)

A questo punto il *SchoolContext* database è stato creato nel Database di SQL Azure di Windows perché è stato selezionato **eseguire migrazioni Code First (esecuzione all'avvio dell'app)**. Il *Web. config* file nel sito web distribuito è stato modificato in modo che il [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inizializzatore verrebbe eseguito la prima volta il codice legge o scrive dati nel database (che si è verificato quando seleziona il **studenti** scheda):

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image33.png)

Il processo di distribuzione creata anche una nuova stringa di connessione *(SchoolContext\_DatabasePublish*) per le migrazioni Code First da utilizzare per l'aggiornamento dello schema del database e il seeding del database.

![Stringa di connessione Database_Publish](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image34.png)

Il *DefaultConnection* stringa di connessione è per il database delle appartenenze (che non vengono utilizzati in questa esercitazione). Il *SchoolContext* stringa di connessione è per il database ContosoUniversity.

È possibile trovare la versione distribuita del file Web. config nel computer in *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. È possibile accedere distribuito *Web. config* file tramite FTP. Per istruzioni, vedere [distribuzione Web ASP.NET utilizzando Visual Studio: distribuzione di un aggiornamento del codice](../../../../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Seguire le istruzioni che iniziano con "per utilizzare uno strumento FTP, sono necessari tre operazioni: l'URL di FTP, il nome utente e la password."

> [!NOTE]
> L'app web non implementa la sicurezza, in modo che tutti gli utenti che consente di trovare l'URL può modificare i dati. Per istruzioni su come proteggere il sito web, vedere [distribuire un'app protetta ASP.NET MVC con appartenenza, OAuth e il Database SQL a un sito Web di Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). È possibile impedire che altri utenti del sito usando il portale di gestione di Windows Azure o **Esplora Server** in Visual Studio per arrestare il sito.


![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image35.png)

## <a name="code-first-initializers"></a>Inizializzatori prima di codice

Nella sezione distribuzione è stato illustrato il [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inizializzatore in uso. Codice innanzitutto fornisce anche altri inizializzatori che è possibile utilizzare, tra cui [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (impostazione predefinita), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) e [ DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). Il `DropCreateAlways` inizializzatore può essere utile per impostare le condizioni per gli unit test. È anche possibile scrivere il propria inizializzatori ed è possibile chiamare un inizializzatore in modo esplicito se non si desidera attendere che l'applicazione legge o scrive nel database. Per una spiegazione completa di inizializzatori, vedere il capitolo 6 del libro [Entity Framework di programmazione: Code First](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman e Miller Rowan.

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato spiegato come creare un modello di data e l'implementazione di base CRUD, ordinamento, filtro, paging e la funzionalità di raggruppamento. Nella prossima esercitazione verrà iniziare esaminando argomenti più avanzati espandendo il modello di dati.

Collegamenti ad altre risorse di Entity Framework, vedere il [mappa del contenuto ASP.NET dati accesso](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Precedente](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [Successivo](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
