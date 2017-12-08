---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: Ordinamento, filtro e Paging con Entity Framework in un'applicazione MVC ASP.NET | Documenti Microsoft
author: tdykstra
description: L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 5 con Code First di Entity Framework 6 e Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/01/2015
ms.topic: article
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 8d11bf47f8c43040ef30d7132f0bb756748dbacd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Ordinamento, filtro e Paging con Entity Framework in un'applicazione MVC ASP.NET
====================
Da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) o [Scarica il PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 5 con Visual Studio 2013 e Code First di Entity Framework 6. Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Nell'esercitazione precedente è stato implementato un set di pagine web per operazioni CRUD di base per `Student` entità. In questa esercitazione si aggiungerà ordinamento, filtro e la funzionalità di paging per il **studenti** pagina di indice. Si creeranno inoltre una pagina che esegue il raggruppamento semplice.

Nella figura seguente viene illustrato l'aspetto di pagina al termine. Le intestazioni di colonna sono link che l'utente può fare clic per ordinare in base alla colonna. Fare clic su una colonna con intestazione ripetutamente passa alternativamente da crescente e decrescente.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Aggiungere collegamenti di ordinamento di colonna per la pagina di indice di studenti

Per aggiungere l'ordinamento alla pagina di indice di studenti, sarà necessario impostare il `Index` metodo il `Student` controller e aggiungere codice per il `Student` indicizzare la vista.

### <a name="add-sorting-functionality-to-the-index-method"></a>Aggiungere funzionalità al metodo di indice di ordinamento

In *Controllers\StudentController.cs*, sostituire il `Index` (metodo) con il codice seguente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Questo codice riceve un `sortOrder` parametro dalla stringa di query nell'URL. Il valore di stringa di query viene fornito da ASP.NET MVC come parametro al metodo di azione. Il parametro sarà una stringa che è "Name" o "Date", seguito facoltativamente da un carattere di sottolineatura e la stringa "desc" per specificare l'ordine decrescente. L'ordinamento predefinito è crescente.

La prima volta che viene richiesta la pagina di indice, viene individuata alcuna stringa di query. Gli studenti vengono visualizzati in ordine crescente in base `LastName`, ovvero il valore predefinito definito dalla istruzione case nel `switch` istruzione. Quando l'utente fa clic hyperlink intestazione di colonna, appropriata `sortOrder` valore viene fornito nella stringa di query.

I due `ViewBag` variabili vengono utilizzate in modo che la vista è possibile configurare i collegamenti ipertestuali sull'intestazione di colonna con i valori di stringa di query appropriata:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Si tratta di istruzioni ternarie. Il primo specifica che se il `sortOrder` parametro è null o vuoto, `ViewBag.NameSortParm` deve essere impostato su "nome\_desc"; in caso contrario, deve essere impostato su una stringa vuota. Le due istruzioni seguenti consentono la visualizzazione impostare la colonna collegamenti ipertestuali di intestazione come indicato di seguito:

| Ordinamento corrente | Collegamento ipertestuale cognome | Collegamento ipertestuale Data |
| --- | --- | --- |
| Ultimo nome (crescente) | descending | ascending |
| Ultimo nome (decrescente) | ascending | ascending |
| Data ordine crescente | ascending | descending |
| Data ordine decrescente | ascending | ascending |

Il metodo utilizza [LINQ to Entities](https://msdn.microsoft.com/en-us/library/bb386964.aspx) per specificare la colonna da ordinare. Il codice crea un [IQueryable](https://msdn.microsoft.com/en-us/library/bb351562.aspx) variabile prima il `switch` istruzione modifica nel `switch` istruzione e chiama il `ToList` metodo dopo il `switch` istruzione. Quando si crea e modifica `IQueryable` variabili, nessuna query viene inviata al database. La query non viene eseguita fino alla conversione di `IQueryable` oggetto in una raccolta chiamando un metodo, ad esempio `ToList`. Pertanto, questo codice genera una singola query che non viene eseguita finché il `return View` istruzione.

In alternativa alla scrittura di istruzioni LINQ diverse per ogni criterio di ordinamento, è possibile creare dinamicamente un'istruzione LINQ. Per informazioni su LINQ dinamica, vedere [dinamiche di LINQ](https://go.microsoft.com/fwlink/?LinkID=323957).

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Aggiungere collegamenti ipertestuali per la visualizzazione dell'indice per studenti di intestazione di colonna

In *Views\Student\Index.cshtml*, sostituire il `<tr>` e `<th>` elementi per la riga di intestazione con il codice evidenziato di seguito:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

Questo codice Usa le informazioni di `ViewBag` valori di stringa di proprietà per impostare i collegamenti ipertestuali con la query appropriato.

Eseguire la pagina e fare clic su di **cognome** e **data di registrazione** funziona intestazioni di colonna per verificare che l'ordinamento.

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Dopo aver selezionato il **Last Name** titolo, gli studenti vengono visualizzati in ultimo nome ordine decrescente.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Aggiungere una casella di ricerca per la pagina di indice di studenti

Per aggiungere filtri alla pagina di indice di studenti, aggiungerai un pulsante di invio e di una casella di testo per la visualizzazione e apportare le modifiche corrispondenti nel `Index` metodo. La casella di testo consente di immettere una stringa da cercare nel nome e cognome.

### <a name="add-filtering-functionality-to-the-index-method"></a>Aggiungere funzionalità di filtro per il metodo di Index

In *Controllers\StudentController.cs*, sostituire il `Index` (metodo) con il codice seguente (le modifiche vengono evidenziate):

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

È stato aggiunto un `searchString` parametro per il `Index` (metodo). Il valore di stringa di ricerca viene ricevuto da una casella di testo che verrà aggiunto per la visualizzazione dell'indice. Anche aggiunte all'istruzione LINQ un `where` clausola che seleziona solo gli studenti il cui nome o cognome contengono la stringa di ricerca. L'istruzione che aggiunge il [dove](https://msdn.microsoft.com/en-us/library/bb535040.aspx) clausola viene eseguita solo se è presente un valore per la ricerca.

> [!NOTE]
> In molti casi è possibile chiamare lo stesso metodo su un set di entità di Entity Framework o come metodo di estensione per una raccolta in memoria. I risultati sono in genere uguali, ma in alcuni casi potrebbero essere diversi.
> 
> Ad esempio, l'implementazione di .NET Framework del `Contains` restituisce tutte le righe quando è passare una stringa vuota, ma il provider di Entity Framework per SQL Server Compact 4.0 restituisce zero righe, le stringhe vuote. Pertanto il codice di esempio (inserisce il `Where` istruzione all'interno di un `if` istruzione) rende è necessario ottenere gli stessi risultati per tutte le versioni di SQL Server. Inoltre, l'implementazione di .NET Framework del `Contains` metodo esegue un confronto tra maiuscole e minuscole per impostazione predefinita, ma per impostazione predefinita, i provider di Entity Framework SQL Server eseguono confronti tra maiuscole e minuscole. Pertanto, la chiamata di `ToUpper` metodo per eseguire il test in modo esplicito con distinzione tra maiuscole e assicura che non cambiano quando si modifica il codice in un secondo momento per utilizzare un repository, che restituirà un `IEnumerable` raccolta invece di un `IQueryable` oggetto. (Quando si chiama il `Contains` metodo su un `IEnumerable` raccolta, si ottiene l'implementazione di .NET Framework; quando si chiama su un `IQueryable` dell'oggetto, si ottiene l'implementazione del provider di database.)
> 
> Gestione di valori null può essere diversa per diversi provider di database o quando si usa un `IQueryable` oggetto rispetto a quando si utilizza un `IEnumerable` insieme. Ad esempio, in alcuni scenari una `Where` condizione, ad esempio `table.Column != 0` non può restituire le colonne contenenti `null` come valore. Per ulteriori informazioni, vedere [gestione non corretta delle variabili null nella clausola 'where'](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl).


### <a name="add-a-search-box-to-the-student-index-view"></a>Aggiungere una casella di ricerca per la visualizzazione dell'indice per studenti

In *Views\Student\Index.cshtml*, aggiungere il codice evidenziato immediatamente prima dell'apertura `table` tag per creare una didascalia, una casella di testo e un **ricerca** pulsante.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Eseguire la pagina, immettere una stringa di ricerca e fare clic su **ricerca** per verificare che il filtro sia funzionante.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Si noti che l'URL non contiene "an" stringa di ricerca, il che significa che se questa pagina, non si otterrà l'elenco filtrato quando si utilizza il segnalibro. Questo vale anche per i collegamenti di ordinamento di colonna, come l'ordinamento sarà l'intero elenco. Si modificherà il **ricerca** pulsante da usare le stringhe di query per i criteri di filtro più avanti nell'esercitazione.

## <a name="add-paging-to-the-students-index-page"></a>Aggiungere il Paging per la pagina di indice di studenti

Per aggiungere il paging per la pagina di indice di studenti, si inizierà installando il **PagedList.Mvc** pacchetto NuGet. Quindi è possibile apportare ulteriori modifiche nel `Index` (metodo) e aggiungere collegamenti di paging per il `Index` visualizzazione. **PagedList.Mvc** è uno dei molti paging buona e ordinamento dei pacchetti per ASP.NET MVC, ed è destinato solo l'uso di seguito un esempio, non come un suggerimento su altre opzioni. Nella figura seguente vengono visualizzati i collegamenti di spostamento.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>Installare il pacchetto PagedList.MVC NuGet

NuGet **PagedList.Mvc** pacchetto installa automaticamente il **PagedList** pacchetto come dipendenza. Il **PagedList** pacchetto installa un `PagedList` metodi di estensione e di tipo di raccolta per `IQueryable` e `IEnumerable` raccolte. I metodi di estensione per creano una singola pagina di dati in un `PagedList` raccolta fuori il `IQueryable` o `IEnumerable`e `PagedList` raccolta fornisce molte proprietà e metodi che facilitano lo spostamento. Il **PagedList.Mvc** pacchetto viene installato un supporto di paging che visualizza i pulsanti di spostamento.

Dal **strumenti** dal menu **Gestione pacchetti libreria** e quindi **Package Manager Console**.

Nel **Package Manager Console** finestra, assicurarsi che il **origine pacchetto** è **nuget.org** e **progetto predefinito** è **ContosoUniversity**, quindi immettere il comando seguente:

`Install-Package PagedList.Mvc`

![Installare PagedList.Mvc](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Compilare il progetto. 

### <a name="add-paging-functionality-to-the-index-method"></a>Aggiungere funzionalità di Paging per il metodo di Index

In *Controllers\StudentController.cs*, aggiungere un `using` istruzione per il `PagedList` dello spazio dei nomi:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Sostituire il metodo `Index` con il codice seguente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

Questo codice aggiunge un `page` parametro, un parametro di ordine di ordinamento corrente e un parametro di filtro corrente per la firma del metodo:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

La prima volta che viene visualizzata la pagina o se l'utente non è stato selezionato un paging o ordinamento di collegamento, tutti i parametri è null. Se si seleziona un collegamento di paging, il `page` variabile conterrà il numero di pagina da visualizzare.

Oggetto `ViewBag` proprietà fornisce la vista con l'ordinamento corrente, in quanto ciò deve essere inclusi i collegamenti di spostamento per mantenere la stessa durante il paging dell'ordinamento:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Un'altra proprietà, `ViewBag.CurrentFilter`, fornisce la visualizzazione con la stringa di filtro corrente. Questo valore deve essere incluso nei collegamenti di spostamento per mantenere le impostazioni di filtro durante il paging, e devono essere ripristinato nella casella di testo quando la pagina viene visualizzata. Se la stringa di ricerca viene modificata durante il paging, la pagina deve essere reimpostato su 1, in quanto può comportare dati diversi per visualizzare il nuovo filtro. La stringa di ricerca viene modificata quando viene immesso un valore nella casella di testo e viene premuto il pulsante Invia. In tal caso, il `searchString` parametro non è null.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Alla fine del metodo, il `ToPagedList` metodo di estensione su studenti `IQueryable` oggetto converte la query di studenti in una singola pagina degli studenti iscritti a un tipo di insieme che supporta il paging. Pagina singola di studenti viene quindi passato alla visualizzazione:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Il `ToPagedList` metodo accetta un numero di pagina. Due punti interrogativi rappresentano il [operatore null-coalescing](https://msdn.microsoft.com/en-us/library/ms173224.aspx). L'operatore null-coalescing definisce un valore predefinito per un tipo nullable. l'espressione `(page ?? 1)` significa restituisca il valore di `page` se ha un valore oppure restituiscono 1 se `page` è null.

### <a name="add-paging-links-to-the-student-index-view"></a>Aggiungere collegamenti di spostamento per la visualizzazione dell'indice per studenti

In *Views\Student\Index.cshtml*, sostituire il codice esistente con il codice seguente. Le modifiche sono evidenziate.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

Il `@model` istruzione nella parte superiore della pagina specifica che la vista Ottiene ora un `PagedList` oggetto anziché un `List` oggetto.

Il `using` istruzione per `PagedList.Mvc` consente di accedere al supporto MVC per i pulsanti di spostamento.

Il codice utilizza un overload di [BeginForm](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) che consente di specificare [FormMethod.Get](https://msdn.microsoft.com/en-us/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

Il valore predefinito [BeginForm](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) invia i dati del form con un POST, il che significa che i parametri vengono passati nel corpo del messaggio HTTP e non nell'URL come stringhe di query. Quando si specifica di HTTP GET, i dati del form viene passati nell'URL come stringhe di query, che consente agli utenti di segnalibro l'URL. Il [W3C linee guida per l'utilizzo di HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) consiglia che, quando l'azione non comporta un aggiornamento, è necessario utilizzare GET.

La casella di testo viene inizializzata con la stringa di ricerca corrente quando si fa clic su una nuova pagina per visualizzare la stringa di ricerca corrente.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

I collegamenti di intestazione di colonna utilizzano la stringa di query per passare la stringa di ricerca corrente al controller in modo che l'utente può ordinare all'interno dei risultati di filtro:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

Viene visualizzato il numero corrente di pagina e il totale di pagine.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

Se non sono presenti pagine per la visualizzazione, viene visualizzata "Pagina 0 0". (In tal caso il numero di pagina è maggiore del numero di pagina perché `Model.PageNumber` è 1, e `Model.PageCount` è 0.)

Vengono visualizzati i pulsanti di spostamento per la `PagedListPager` helper:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

Il `PagedListPager` helper fornisce una serie di opzioni che è possibile personalizzare, inclusi URL e lo stile. Per ulteriori informazioni, vedere [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) sul sito GitHub.

Eseguire la pagina.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Fare clic sui collegamenti di spostamento in diversi tipi di ordinamento per assicurarsi che funziona di paging. Quindi immettere una stringa di ricerca e tentare di paging per verificare che il paging funziona inoltre correttamente con ordinamento e filtro.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Creare una pagina che mostra le statistiche di Student

Per Contoso University del sito Web sulla pagina, verranno visualizzati il numero di studenti sono registrati per ciascuna data di registrazione. Questa operazione richiede i calcoli di raggruppamento e semplici ai gruppi. A tale scopo, verranno eseguite le operazioni seguenti:

- Creare una classe di modello di visualizzazione per i dati che è necessario passare alla visualizzazione.
- Modificare il `About` metodo il `Home` controller.
- Modificare il `About` visualizzazione.

### <a name="create-the-view-model"></a>Creare il modello di visualizzazione

Creare un *ViewModel* cartella nella cartella del progetto. In tale cartella, aggiungere un file di classe *EnrollmentDateGroup.cs* e sostituire il codice del modello con il codice seguente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Modificare il Controller Home

In *HomeController.cs*, aggiungere il seguente `using` istruzioni nella parte superiore del file:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Aggiungere una variabile di classe per il contesto del database immediatamente dopo la parentesi graffa di apertura per la classe:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

Sostituire il metodo `About` con il codice seguente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

Raggruppa le entità di studenti per data di registrazione dell'istruzione LINQ, calcola il numero di entità in ciascun gruppo e archivia i risultati in una raccolta di `EnrollmentDateGroup` consente di visualizzare gli oggetti del modello.

Aggiungere un `Dispose` metodo:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Modificare le informazioni sulla visualizzazione

Sostituire il codice di *Views\Home\About.cshtml* file con il codice seguente:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

Eseguire l'app e fare clic su di **su** collegamento. Il numero di studenti per ogni data di registrazione viene visualizzato in una tabella.

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato spiegato come creare un modello di data e l'implementazione di base CRUD, ordinamento, filtro, paging e la funzionalità di raggruppamento. Nella prossima esercitazione verrà iniziare esaminando argomenti più avanzati espandendo il modello di dati.

Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione ed è stato possibile apportare miglioramenti. È inoltre possibile richiedere nuovi argomenti in [Mostra Me come con codice](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Sono disponibili collegamenti ad altre risorse di Entity Framework in [accesso ai dati ASP.NET - risorse](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Precedente](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
[Successivo](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)
