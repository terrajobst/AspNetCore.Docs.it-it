---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Esercitazione: Aggiungere l'ordinamento, filtro e paging con Entity Framework in un'applicazione ASP.NET MVC | Microsoft Docs"
author: tdykstra
description: In questa esercitazione aggiungere l'ordinamento, filtro e la funzionalità di paging per il **studenti** pagina di indice. È anche possibile creare una pagina di raggruppamento semplice.
ms.author: riande
ms.date: 01/14/2019
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 1f18a15d39d58ffb4ac48cfccee6519d33294e85
ms.sourcegitcommit: 728f4e47be91e1c87bb7c0041734191b5f5c6da3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2019
ms.locfileid: "54444195"
---
# <a name="tutorial-add-sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Esercitazione: Aggiungere l'ordinamento, filtro e paging con Entity Framework in un'applicazione ASP.NET MVC

Nel [esercitazione precedente](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md), è stato implementato un set di pagine web per operazioni CRUD di base per `Student` entità. In questa esercitazione aggiungere l'ordinamento, filtro e la funzionalità di paging per il **studenti** pagina di indice. È anche possibile creare una pagina di raggruppamento semplice.

L'immagine seguente mostra l'aspetto di pagina al termine. Le intestazioni di colonna sono collegamenti su cui l'utente può fare clic per eseguire l'ordinamento in base alla colonna. Facendo clic più volte su un'intestazione di colonna è possibile passare dall'ordinamento crescente a quello decrescente e viceversa.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Aggiungere collegamenti di ordinamento di colonna
> * Aggiungere una casella di ricerca
> * Aggiunta di impaginazione
> * Creare una pagina About

## <a name="prerequisites"></a>Prerequisiti

* [Implementazione della funzionalità CRUD di base](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)

## <a name="add-column-sort-links"></a>Aggiungere collegamenti di ordinamento di colonna

Per aggiungere l'ordinamento alla pagina di indice degli studenti, sarà necessario modificare il `Index` metodo per il `Student` controller e aggiungere codice al `Student` indicizzare la vista.

### <a name="add-sorting-functionality-to-the-index-method"></a>Aggiungere funzionalità di ordinamento al metodo Index

- Nelle *Controllers\StudentController.cs*, sostituire il `Index` metodo con il codice seguente:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Questo codice riceve un parametro `sortOrder` dalla stringa di query nell'URL. Il valore di stringa di query viene fornito da ASP.NET MVC come parametro al metodo di azione. Il parametro è una stringa che può essere "Name" o "Date", seguito facoltativamente da un carattere di sottolineatura e dalla stringa "desc" per specificare l'ordine decrescente. L'ordinamento predefinito è crescente.

La prima volta che viene richiesta la pagina di indice, non è presente alcuna stringa di query. Gli studenti vengono visualizzati in ordine crescente in base `LastName`, ovvero il valore predefinito come stabilito dal caso di FallThrough nel `switch` istruzione. Quando l'utente fa clic sul collegamento ipertestuale di un'intestazione di colonna, nella stringa di query viene specificato il valore `sortOrder` appropriato.

I due `ViewBag` variabili vengono usate in modo che la visualizzazione è possibile configurare i collegamenti ipertestuali sull'intestazione di colonna con i valori di stringa di query appropriata:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Si tratta di istruzioni ternarie. La prima specifica che se il `sortOrder` parametro è null o vuoto, `ViewBag.NameSortParm` deve essere impostato su "nome\_desc"; in caso contrario, deve essere impostato su una stringa vuota. Queste due istruzioni consentono alla visualizzazione di impostare i collegamenti ipertestuali dell'intestazione di colonna come indicato di seguito:

| Ordinamento corrente | Collegamento ipertestuale cognome | Collegamento ipertestuale data |
| --- | --- | --- |
| Cognome in ordine crescente | descending | ascending |
| Cognome in ordine decrescente | ascending | ascending |
| Data in ordine crescente | ascending | descending |
| Data in ordine decrescente | ascending | ascending |

Il metodo Usa [LINQ to Entities](/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities) per specificare la colonna da ordinare. Il codice crea un' <xref:System.Linq.IQueryable%601> variabile prima il `switch` istruzione, lo modifica nel `switch` istruzione e chiama il `ToList` metodo dopo il `switch` istruzione. Quando si creano e modificano variabili `IQueryable`, nessuna query viene inviata al database. La query non viene eseguita fino a quando non si converte il `IQueryable` oggetti in una raccolta chiamando un metodo, ad esempio `ToList`. Di conseguenza, questo codice genera una singola query che non viene eseguita finché il `return View` istruzione.

Come alternativa alla scrittura di istruzioni LINQ diversi per ogni ordine di ordinamento, è possibile creare dinamicamente un'istruzione LINQ. Per informazioni su LINQ dinamiche, vedere [LINQ dinamiche](https://go.microsoft.com/fwlink/?LinkID=323957).

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Aggiungere collegamenti ipertestuali delle intestazioni di colonna per la visualizzazione dell'indice degli studenti

1. Nelle *Views\Student\Index.cshtml*, sostituire il `<tr>` e `<th>` elementi per la riga di intestazione con il codice evidenziato:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

   Questo codice Usa le informazioni contenute nel `ViewBag` valori stringa delle proprietà per impostare i collegamenti ipertestuali con la query appropriato.

2. Eseguire la pagina, quindi scegliere il **Last Name** e **data di registrazione** funziona le intestazioni di colonna per verificare che l'ordinamento.

   Dopo aver selezionato il **cognome** intestazione studenti vengono visualizzati nell'ultimo nome ordine decrescente.

## <a name="add-a-search-box"></a>Aggiungere una casella di ricerca

Per aggiungere filtri alla pagina di indice degli studenti, verrà aggiunto alla visualizzazione una casella di testo e un pulsante di invio e apportare le modifiche corrispondenti nel `Index` (metodo). La casella di testo consente di immettere una stringa da cercare nel nome e cognome.

### <a name="add-filtering-functionality-to-the-index-method"></a>Aggiungere la funzionalità di filtro al metodo Index

- Nelle *Controllers\StudentController.cs*, sostituire il `Index` metodo con il codice seguente (le modifiche sono evidenziate):

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

Il codice aggiunge un `searchString` parametro per il `Index` (metodo). Il valore della stringa di ricerca viene ricevuto da una casella di testo che verrà aggiunta alla visualizzazione Index (Indice). Aggiunge anche un `where` clausola all'istruzione LINQ che seleziona solo gli studenti il cui nome o cognome contengono la stringa di ricerca. L'istruzione che aggiunge il <xref:System.Linq.Queryable.Where%2A> clausola viene eseguita solo se è previsto un valore per la ricerca.

> [!NOTE]
> In molti casi è possibile chiamare il metodo di stesso in un set di entità di Entity Framework o come metodo di estensione per una raccolta in memoria. I risultati sono in genere gli stessi ma in alcuni casi potrebbero essere diversi.
>
> Ad esempio, l'implementazione di .NET Framework del `Contains` metodo restituisce tutte le righe quando si passa una stringa vuota a esso, ma il provider di Entity Framework per SQL Server Compact 4.0 restituisce zero righe per le stringhe vuote. Di conseguenza il codice nell'esempio (inserire la `Where` istruzione all'interno di un `if` istruzione) garantisce che ottenere gli stessi risultati per tutte le versioni di SQL Server. Inoltre, l'implementazione di .NET Framework del `Contains` metodo esegue un confronto tra maiuscole e minuscole per impostazione predefinita, ma i provider di Entity Framework SQL Server eseguono confronti tra maiuscole e minuscole per impostazione predefinita. Pertanto, la chiamata di `ToUpper` metodo in modo che il test in modo esplicito tra maiuscole e minuscole assicura che non cambiano quando si modifica il codice in un secondo momento per usare un repository, che restituirà un `IEnumerable` raccolta anziché un `IQueryable` oggetto. Quando il metodo `Contains` viene chiamato su una raccolta `IEnumerable`, si ottiene l'implementazione di .NET Framework; quando viene chiamato su un oggetto `IQueryable`, si ottiene l'implementazione del provider di database.
>
> Gestione dei valori null può essere diversa per diversi provider di database o quando si usa un' `IQueryable` oggetto rispetto a quando si usa un `IEnumerable` raccolta. Ad esempio, in alcuni scenari una `Where` condizione, ad esempio `table.Column != 0` potrebbe non restituire le colonne contenenti `null` come valore. Per altre informazioni, vedere [errate di gestione di variabili null nella clausola 'where'](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl).

### <a name="add-a-search-box-to-the-student-index-view"></a>Aggiungere una casella di ricerca per la visualizzazione dell'indice degli studenti

1. Nelle *Views\Student\Index.cshtml*, aggiungere il codice evidenziato immediatamente prima dell'apertura `table` tag per creare una didascalia, una casella di testo e un **ricerca** pulsante.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=4-11)]

2. Esecuzione della pagina, immettere una stringa di ricerca e fare clic su **ricerca** per verificare che il filtro funzioni.

   Si noti che l'URL non contiene "an" stringa di ricerca, il che significa che se si contrassegna pagina, si otterranno l'elenco filtrato quando si usa il segnalibro. Questo vale anche per i collegamenti di ordinamento di colonna, come che verranno ordinare l'intero elenco. Sarà necessario modificare il **ricerca** pulsante da usare le stringhe di query per i criteri di filtro più avanti nell'esercitazione.

## <a name="add-paging"></a>Aggiunta di impaginazione

Per aggiungere paging alla pagina di indice degli studenti, si inizia installando i **PagedList.Mvc** pacchetto NuGet. È quindi possibile apportare altre modifiche nel `Index` metodo e aggiungere collegamenti di paging per il `Index` vista. **PagedList.Mvc** è una delle molte buone paging e ordinamento dei pacchetti di ASP.NET MVC, ed è destinato solo l'uso di seguito un esempio, non come una raccomandazione per tale su altre opzioni.

### <a name="install-the-pagedlistmvc-nuget-package"></a>Installare il pacchetto PagedList.MVC NuGet

NuGet **PagedList.Mvc** pacchetto installa automaticamente il **PagedList** pacchetto come dipendenza. Il **PagedList** pacchetto installa un `PagedList` metodi di estensione e tipo di raccolta per `IQueryable` e `IEnumerable` raccolte. I metodi di estensione creano una singola pagina di dati in un `PagedList` raccolta fuori il `IQueryable` o `IEnumerable`e il `PagedList` raccolta fornisce molte proprietà e metodi che facilitano il paging. Il **PagedList.Mvc** pacchetto viene installato un supporto di paging che visualizza i pulsanti di spostamento.

1. Dal **degli strumenti** dal menu **Gestione pacchetti NuGet** e quindi **Package Manager Console**.

2. Nel **Console di gestione pacchetti** finestra, assicurarsi che il **origine pacchetto** viene **nuget.org** e il **progetto predefinito** è **ContosoUniversity**, quindi immettere il comando seguente:

   ```text
   Install-Package PagedList.Mvc
   ```

3. Compilare il progetto.

### <a name="add-paging-functionality-to-the-index-method"></a>Aggiungere la funzionalità di suddivisione in pagine al metodo Index

1. Nelle *Controllers\StudentController.cs*, aggiungere un `using` istruzione per il `PagedList` dello spazio dei nomi:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

2. Sostituire il metodo `Index` con il codice seguente:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

   Questo codice aggiunge un `page` parametro, un parametro di ordinamento corrente e un parametro di filtro corrente alla firma del metodo:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

   La prima volta che viene visualizzata la pagina o se l'utente non ha selezionato una suddivisione in pagine o collegamento di ordinamento, tutti i parametri sono null. Se viene selezionato un collegamento di paging, il `page` variabile contiene il numero di pagina da visualizzare.

   Oggetto `ViewBag` proprietà offre la visualizzazione con l'ordinamento corrente, in quanto esso deve essere incluso nei collegamenti di paging in grado di mantenere lo stesso di suddivisione in pagine all'interno dell'ordinamento:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

   Un'altra proprietà, `ViewBag.CurrentFilter`, fornisce una vista con la stringa di filtro corrente. Questo valore deve essere incluso nei collegamenti di suddivisione in pagine per mantenere le impostazioni di filtro nella suddivisione in pagine e deve essere ripristinato nella casella di testo quando la pagina viene nuovamente visualizzata. Se la stringa di ricerca viene modificata nella suddivisione in pagine, la pagina deve essere reimpostata su 1, poiché il nuovo filtro può comportare la visualizzazione di dati diversi. La stringa di ricerca viene modificata quando viene immesso un valore nella casella di testo e viene premuto il pulsante di invio. In tal caso, il `searchString` parametro non è null.

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

   Alla fine del metodo, il `ToPagedList` metodo di estensione su studenti `IQueryable` oggetto converte la query degli studenti in una pagina singola di studenti in un tipo di raccolta che supporta il paging. La pagina singola di studenti viene quindi passata alla visualizzazione:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

   Il metodo `ToPagedList` accetta un numero di pagina. I due punti interrogativi rappresentano il [operatore null-coalescing](/dotnet/csharp/language-reference/operators/null-coalescing-operator). L'operatore null-coalescing definisce un valore predefinito per un tipo nullable. L'espressione `(page ?? 1)` significa restituzione del valore di `page` se ha un valore oppure restituzione di 1 se `page` è Null.

### <a name="add-paging-links-to-the-student-index-view"></a>Aggiungere collegamenti di paging per la visualizzazione dell'indice degli studenti

1. Nelle *Views\Student\Index.cshtml*, sostituire il codice esistente con il codice seguente. Le modifiche vengono evidenziate.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

   L'istruzione `@model` nella parte superiore della pagina specifica che la vista ottiene ora un oggetto `PagedList` anziché un oggetto `List`.

   Il `using` istruzione per `PagedList.Mvc` concede l'accesso al supporto MVC per i pulsanti di spostamento.

   Il codice Usa un overload del [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) che consente di specificare [FormMethod.Get](/previous-versions/aspnet/dd460179(v=vs.100)).

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

   Il valore predefinito [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) invia i dati del modulo con POST, il che significa che i parametri vengono passati nel corpo del messaggio HTTP e non nell'URL come stringhe di query. Quando si specifica HTTP GET, i dati del modulo vengono passati nell'URL come stringhe di query, il che consente agli utenti di inserire l'URL tra i segnalibri. Il [linee guida W3C per l'uso di HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) consiglia che è necessario usare GET quando l'azione non comporta un aggiornamento.

   La casella di testo viene inizializzata con la stringa di ricerca corrente in modo che quando si fa clic su una nuova pagina è possibile visualizzare la stringa di ricerca corrente.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

   I collegamenti delle intestazioni di colonna usano la stringa di query per passare la stringa di ricerca corrente al controller in modo che l'utente possa procedere all'ordinamento all'interno dei risultati di filtro:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

   Viene visualizzato il numero corrente di pagina e il totale di pagine.

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

   Se non sono presenti Nessuna pagina da visualizzare, viene visualizzata "Pagina 0 0". (In questo caso il numero di pagina è maggiore del numero di pagina in quanto `Model.PageNumber` è 1, e `Model.PageCount` è 0.)

   I pulsanti di spostamento vengono visualizzati per il `PagedListPager` helper:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

   Il `PagedListPager` helper offre numerose opzioni che è possibile personalizzare, inclusi URL e stile. Per altre informazioni, vedere [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) sul sito GitHub.

2. Eseguire la pagina.

   Fare clic sui collegamenti di suddivisione in pagine in diversi tipi di ordinamento per verificare che la suddivisione in pagine funzioni. Immettere quindi una stringa di ricerca e provare nuovamente la suddivisione in pagine per verificare che funzioni correttamente anche con l'ordinamento e il filtro.

## <a name="create-an-about-page"></a>Creare una pagina About

Per il sito Web Contoso University sulla pagina, verranno visualizzati il numero di studenti iscritti per ogni data di registrazione. Questa operazione richiede calcoli di raggruppamento e semplici sui gruppi. Per completare questa procedura, è necessario eseguire le operazioni seguenti:

- Creare una classe modello di visualizzazione per i dati che è necessario passare alla visualizzazione.
- Modificare il `About` nel metodo il `Home` controller.
- Modificare il `About` vista.

### <a name="create-the-view-model"></a>Creare il modello di visualizzazione

Creare un *ViewModel* cartella nella cartella del progetto. In tale cartella, aggiungere un file di classe *EnrollmentDateGroup.cs* e sostituire il codice del modello con il codice seguente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Modificare il controller Home

1. Nelle *HomeController.cs*, aggiungere il codice seguente `using` istruzioni all'inizio del file:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

2. Aggiungere una variabile di classe per il contesto del database immediatamente dopo la parentesi graffa di apertura per la classe:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

3. Sostituire il metodo `About` con il codice seguente:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

   L'istruzione LINQ raggruppa le entità di studenti per data di registrazione, calcola il numero di entità in ogni gruppo e archivia i risultati in una raccolta di oggetti di modello della visualizzazione `EnrollmentDateGroup`.

4. Aggiungere un `Dispose` metodo:

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Modificare la visualizzazione della pagina About (Informazioni)

1. Sostituire il codice nel *Views\Home\About.cshtml* file con il codice seguente:

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

2. Eseguire l'app, quindi scegliere il **sulle** collegamento.

   Consente di visualizzare il numero di studenti per ogni data di registrazione in una tabella.

   ![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="get-the-code"></a>Ottenere il codice

[Scaricare il progetto completato](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

## <a name="additional-resources"></a>Risorse aggiuntive

Collegamenti ad altre risorse di Entity Framework sono disponibili nel [l'accesso ai dati ASP.NET - risorse consigliate](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Passaggi successivi

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Aggiungere collegamenti di ordinamento di colonna
> * Aggiungere una casella di ricerca
> * Aggiunta di impaginazione
> * Creare una pagina About

Passare all'articolo successivo per informazioni su come usare l'intercettazione di comando e la resilienza di connessione.
> [!div class="nextstepaction"]
> [Intercettazione di comando e la resilienza di connessione](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)