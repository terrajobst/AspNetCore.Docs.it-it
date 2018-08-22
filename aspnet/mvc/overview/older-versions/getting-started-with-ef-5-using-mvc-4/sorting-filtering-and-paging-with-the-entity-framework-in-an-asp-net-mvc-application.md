---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: L'ordinamento, filtro e Paging con Entity Framework in un'applicazione ASP.NET MVC (3 di 10) | Microsoft Docs
author: tdykstra
description: L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 8af630e0-fffa-4110-9eca-c96e201b2724
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 72180686ea1ee6f6695b24fcedf3af90c1eddcfa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827318"
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application-3-of-10"></a>L'ordinamento, filtro e Paging con Entity Framework in un'applicazione ASP.NET MVC (3 di 10)
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto completato](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio 2012. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). È possibile avviare la serie di esercitazioni a partire dall'inizio oppure [scarica un progetto iniziale per questo capitolo](building-the-ef5-mvc4-chapter-downloads.md) e iniziare da qui.
> 
> > [!NOTE] 
> > 
> > Se si verifica un problema è possibile risolvere, [Scarica il capitolo completato](building-the-ef5-mvc4-chapter-downloads.md) e provare a riprodurre il problema. Confrontando il codice per il codice completo è generalmente possibile trovare la soluzione al problema. Per alcuni errori comuni e come risolverli, vedere [errori e soluzioni alternative.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Nell'esercitazione precedente è stato implementato un set di pagine web per operazioni CRUD di base per `Student` entità. In questa esercitazione si aggiungerà l'ordinamento, filtro e la funzionalità di paging per il **studenti** pagina di indice. Verrà anche creata una pagina che esegue il raggruppamento semplice.

La figura seguente illustra l'aspetto della pagina al termine dell'operazione. Le intestazioni di colonna sono collegamenti su cui l'utente può fare clic per eseguire l'ordinamento in base alla colonna. Facendo clic più volte su un'intestazione di colonna è possibile passare dall'ordinamento crescente a quello decrescente e viceversa.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Aggiungere collegamenti per l'ordinamento di colonna alla pagina Student Index (Indice degli studenti)

Per aggiungere l'ordinamento alla pagina di indice degli studenti, sarà necessario modificare il `Index` metodo per il `Student` controller e aggiungere codice al `Student` indicizzare la vista.

### <a name="add-sorting-functionality-to-the-index-method"></a>Aggiungere funzionalità al metodo Index di ordinamento

Nelle *Controllers\StudentController.cs*, sostituire il `Index` metodo con il codice seguente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Questo codice riceve un parametro `sortOrder` dalla stringa di query nell'URL. Il valore di stringa di query viene fornito da ASP.NET MVC come parametro al metodo di azione. Il parametro sarà una stringa "Name" o "Date", seguito facoltativamente da un carattere di sottolineatura e dalla stringa "desc" per specificare l'ordine decrescente. L'ordinamento predefinito è crescente.

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

Il metodo Usa [LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx) per specificare la colonna da ordinare. Il codice crea un' [IQueryable](https://msdn.microsoft.com/library/bb351562.aspx) variabile prima il `switch` istruzione, lo modifica nel `switch` istruzione e chiama il `ToList` metodo dopo il `switch` istruzione. Quando si creano e modificano variabili `IQueryable`, nessuna query viene inviata al database. La query non viene eseguita fino a quando non si converte il `IQueryable` oggetti in una raccolta chiamando un metodo, ad esempio `ToList`. Di conseguenza, questo codice genera una singola query che non viene eseguita finché il `return View` istruzione.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Aggiungere collegamenti ipertestuali per la visualizzazione dell'indice degli studenti di intestazione di colonna

Nelle *Views\Student\Index.cshtml*, sostituire il `<tr>` e `<th>` elementi per la riga di intestazione con il codice evidenziato:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

Questo codice Usa le informazioni contenute nel `ViewBag` valori stringa delle proprietà per impostare i collegamenti ipertestuali con la query appropriato.

Eseguire la pagina, quindi scegliere il **Last Name** e **data di registrazione** funziona le intestazioni di colonna per verificare che l'ordinamento.

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Dopo aver selezionato il **cognome** intestazione studenti vengono visualizzati nell'ultimo nome ordine decrescente.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Aggiungere una casella di ricerca alla pagina di indice degli studenti

Per aggiungere filtri alla pagina Student Index (Indice degli studenti), è necessario aggiungere alla visualizzazione una casella di testo e un pulsante di invio e apportare le modifiche corrispondenti nel metodo `Index`. La casella di testo consente di immettere una stringa per eseguire la ricerca nei campi di nome e cognome.

### <a name="add-filtering-functionality-to-the-index-method"></a>Aggiungere funzionalità di filtro al metodo Index

Nelle *Controllers\StudentController.cs*, sostituire il `Index` metodo con il codice seguente (le modifiche sono evidenziate):

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

È stato aggiunto un parametro `searchString` al metodo `Index`. È stata anche aggiunta all'istruzione LINQ una `where` clausethat seleziona solo gli studenti il cui nome o cognome contengono la stringa di ricerca. Il valore di stringa di ricerca viene ricevuto da una casella di testo da aggiungere alla vista Index. L'istruzione che aggiunge il [in cui](https://msdn.microsoft.com/library/bb535040.aspx) clausola viene eseguita solo se è presente un valore per la ricerca.

> [!NOTE]
> In molti casi è possibile chiamare il metodo di stesso in un set di entità di Entity Framework o come metodo di estensione per una raccolta in memoria. I risultati sono in genere gli stessi ma in alcuni casi potrebbero essere diversi. Ad esempio, l'implementazione di .NET Framework del `Contains` metodo restituisce tutte le righe quando si passa una stringa vuota a esso, ma il provider di Entity Framework per SQL Server Compact 4.0 restituisce zero righe per le stringhe vuote. Di conseguenza il codice nell'esempio (inserire la `Where` istruzione all'interno di un `if` istruzione) garantisce che ottenere gli stessi risultati per tutte le versioni di SQL Server. Inoltre, l'implementazione di .NET Framework del `Contains` metodo esegue un confronto tra maiuscole e minuscole per impostazione predefinita, ma i provider di Entity Framework SQL Server eseguono confronti tra maiuscole e minuscole per impostazione predefinita. Pertanto, la chiamata di `ToUpper` metodo in modo che il test in modo esplicito tra maiuscole e minuscole assicura che non cambiano quando si modifica il codice in un secondo momento per usare un repository, che restituirà un `IEnumerable` raccolta anziché un `IQueryable` oggetto. Quando il metodo `Contains` viene chiamato su una raccolta `IEnumerable`, si ottiene l'implementazione di .NET Framework; quando viene chiamato su un oggetto `IQueryable`, si ottiene l'implementazione del provider di database.


### <a name="add-a-search-box-to-the-student-index-view"></a>Aggiungere una casella di ricerca alla visualizzazione Student Index (Indice degli studenti)

Nelle *Views\Student\Index.cshtml*, aggiungere il codice evidenziato immediatamente prima dell'apertura `table` tag per creare una didascalia, una casella di testo e un **ricerca** pulsante.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Esecuzione della pagina, immettere una stringa di ricerca e fare clic su **ricerca** per verificare che il filtro funzioni.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Si noti che l'URL non contiene "an" stringa di ricerca, il che significa che se si contrassegna pagina, si otterranno l'elenco filtrato quando si usa il segnalibro. Sarà necessario modificare il **ricerca** pulsante da usare le stringhe di query per i criteri di filtro più avanti nell'esercitazione.

## <a name="add-paging-to-the-students-index-page"></a>Aggiungere Paging alla pagina di indice degli studenti

Per aggiungere paging alla pagina Students Index, si inizia installando i **PagedList.Mvc** pacchetto NuGet. È quindi possibile apportare altre modifiche nel `Index` metodo e aggiungere collegamenti di paging per il `Index` vista. **PagedList.Mvc** è una delle molte buone paging e ordinamento dei pacchetti di ASP.NET MVC, ed è destinato solo l'uso di seguito un esempio, non come una raccomandazione per tale su altre opzioni. La figura seguente mostra i collegamenti di paging.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>Installare il pacchetto PagedList.MVC NuGet

NuGet **PagedList.Mvc** pacchetto installa automaticamente il **PagedList** pacchetto come dipendenza. Il **PagedList** pacchetto installa un `PagedList` metodi di estensione e tipo di raccolta per `IQueryable` e `IEnumerable` raccolte. I metodi di estensione creano una singola pagina di dati in un `PagedList` raccolta fuori il `IQueryable` o `IEnumerable`e il `PagedList` raccolta fornisce molte proprietà e metodi che facilitano il paging. Il **PagedList.Mvc** pacchetto viene installato un supporto di paging che visualizza i pulsanti di spostamento.

Dal **degli strumenti** dal menu **Library Package Manager** e quindi **Gestisci pacchetti NuGet per la soluzione**.

Nel **Gestisci pacchetti NuGet** finestra di dialogo, fare clic sui **Online** scheda a sinistra e quindi immettere "paging" nella casella di ricerca. Quando viene visualizzato il **PagedList.Mvc** del pacchetto, fare clic su **installare**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Nel **Seleziona progetti** fare clic su **OK**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="add-paging-functionality-to-the-index-method"></a>Aggiungere funzionalità di Paging al metodo Index

Nelle *Controllers\StudentController.cs*, aggiungere un `using` istruzione per il `PagedList` dello spazio dei nomi:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Sostituire il metodo `Index` con il codice seguente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Questo codice aggiunge un `page` parametro, un parametro di ordinamento corrente e un parametro di filtro corrente alla firma del metodo, come illustrato di seguito:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

La prima volta che viene visualizzata la pagina o se l'utente non ha selezionato un collegamento di suddivisione in pagine o di ordinamento, tutti i parametri saranno Null. Se viene selezionato un collegamento di paging, il `page` variabile conterrà il numero di pagina da visualizzare.

`A ViewBag` proprietà offre la visualizzazione con l'ordinamento corrente, in quanto esso deve essere incluso nei collegamenti di paging in grado di mantenere lo stesso di suddivisione in pagine all'interno dell'ordinamento:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Un'altra proprietà, `ViewBag.CurrentFilter`, fornisce una vista con la stringa di filtro corrente. Questo valore deve essere incluso nei collegamenti di suddivisione in pagine per mantenere le impostazioni di filtro nella suddivisione in pagine e deve essere ripristinato nella casella di testo quando la pagina viene nuovamente visualizzata. Se la stringa di ricerca viene modificata nella suddivisione in pagine, la pagina deve essere reimpostata su 1, poiché il nuovo filtro può comportare la visualizzazione di dati diversi. La stringa di ricerca viene modificata quando viene immesso un valore nella casella di testo e viene premuto il pulsante di invio. In tal caso, il `searchString` parametro non è null.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Alla fine del metodo, il `ToPagedList` metodo di estensione su studenti `IQueryable` oggetto converte la query degli studenti in una pagina singola di studenti in un tipo di raccolta che supporta il paging. La pagina singola di studenti viene quindi passata alla visualizzazione:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Il metodo `ToPagedList` accetta un numero di pagina. I due punti interrogativi rappresentano il [operatore null-coalescing](https://msdn.microsoft.com/library/ms173224.aspx). L'operatore null-coalescing definisce un valore predefinito per un tipo nullable. L'espressione `(page ?? 1)` significa restituzione del valore di `page` se ha un valore oppure restituzione di 1 se `page` è Null.

### <a name="add-paging-links-to-the-student-index-view"></a>Aggiungere collegamenti di Paging per la visualizzazione dell'indice degli studenti

Nelle *Views\Student\Index.cshtml*, sostituire il codice esistente con il codice seguente:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=6,9,14-20,56-58)]

L'istruzione `@model` nella parte superiore della pagina specifica che la vista ottiene ora un oggetto `PagedList` anziché un oggetto `List`.

Il `using` istruzione per `PagedList.Mvc` concede l'accesso al supporto MVC per i pulsanti di spostamento.

Il codice Usa un overload del [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) che consente di specificare [FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

Il valore predefinito [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) invia i dati del modulo con POST, il che significa che i parametri vengono passati nel corpo del messaggio HTTP e non nell'URL come stringhe di query. Quando si specifica HTTP GET, i dati del modulo vengono passati nell'URL come stringhe di query, il che consente agli utenti di inserire l'URL tra i segnalibri. Il [linee guida W3C per l'uso di HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) specificare che è necessario usare GET quando l'azione non comporta un aggiornamento.

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

Eseguire la pagina.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Fare clic sui collegamenti di suddivisione in pagine in diversi tipi di ordinamento per verificare che la suddivisione in pagine funzioni. Immettere quindi una stringa di ricerca e provare nuovamente la suddivisione in pagine per verificare che funzioni correttamente anche con l'ordinamento e il filtro.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Creare una pagina che mostra le statistiche degli studenti

Per il sito Web Contoso University sulla pagina, verranno visualizzati il numero di studenti iscritti per ogni data di registrazione. Questa operazione richiede calcoli di raggruppamento e semplici sui gruppi. Per completare questa procedura, è necessario eseguire le operazioni seguenti:

- Creare una classe modello di visualizzazione per i dati che è necessario passare alla visualizzazione.
- Modificare il `About` nel metodo il `Home` controller.
- Modificare il `About` vista.

### <a name="create-the-view-model"></a>Creare il modello di visualizzazione

Creare un *ViewModel* cartella. In tale cartella, aggiungere un file di classe *EnrollmentDateGroup.cs* e sostituire il codice esistente con il codice seguente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Modificare il controller Home

Nelle *HomeController.cs*, aggiungere il codice seguente `using` istruzioni all'inizio del file:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Aggiungere una variabile di classe per il contesto del database immediatamente dopo la parentesi graffa di apertura per la classe:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

Sostituire il metodo `About` con il codice seguente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

L'istruzione LINQ raggruppa le entità di studenti per data di registrazione, calcola il numero di entità in ogni gruppo e archivia i risultati in una raccolta di oggetti di modello della visualizzazione `EnrollmentDateGroup`.

Aggiungere un `Dispose` metodo:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Modificare la visualizzazione della pagina About (Informazioni)

Sostituire il codice nel *Views\Home\About.cshtml* file con il codice seguente:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

Eseguire l'app, quindi scegliere il **sulle** collegamento. Il numero di studenti per ogni data di registrazione viene visualizzato in una tabella.

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

## <a name="optional-deploy-the-app-to-windows-azure"></a>Facoltativo: Distribuire l'app in Windows Azure

Finora l'applicazione è stato in esecuzione in locale in IIS Express nel computer di sviluppo. Per renderlo disponibile ad altri utenti per l'utilizzo su Internet, è necessario distribuirla in un provider di hosting web. In questa sezione facoltativa dell'esercitazione è possibile distribuirlo a un sito Web di Microsoft Azure.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Con migrazioni Code First per distribuire il Database

Per distribuire il database si userà migrazioni Code First. Quando si crea il profilo di pubblicazione che consente di configurare le impostazioni per la distribuzione da Visual Studio, si selezionano una casella di controllo etichettato **Esegui migrazioni Code First (inizia all'avvio dell'applicazione)**. Questa impostazione fa in modo che il processo di distribuzione configurare automaticamente l'applicazione *Web. config* del file nel server di destinazione in modo che usa Code First di `MigrateDatabaseToLatestVersion` classe inizializzatore.

Visual Studio non ha alcun effetto con il database durante il processo di distribuzione. Quando l'applicazione distribuita accede al database per la prima volta dopo la distribuzione, Code First automaticamente crea il database o aggiorna lo schema del database alla versione più recente. Se l'applicazione implementa un migrazioni `Seed` metodo, l'esecuzione del metodo dopo la creazione del database o lo schema viene aggiornato.

Le migrazioni `Seed` metodo inserisce i dati di test. Se si distribuisse un ambiente di produzione, è necessario modificare il `Seed` metodo in modo che non solo inserisce i dati che si desidera essere inseriti nel database di produzione. Ad esempio, il modello di dati corrente è consigliabile avere corsi reali ma fittizi studenti nel database di sviluppo. È possibile scrivere un `Seed` metodo caricare entrambi in fase di sviluppo e quindi impostare come commento fittizi studenti prima di distribuire nell'ambiente di produzione. In alternativa è possibile scrivere un `Seed` metodo per caricare solo i corsi e immettere gli studenti fittizi nel database di test manualmente tramite interfaccia utente dell'applicazione.

### <a name="get-a-windows-azure-account"></a>Ottenere un account di Windows Azure

È necessario un account di Azure. Se non si ha già uno, è possibile creare un account di valutazione gratuito in pochi minuti. Per informazioni dettagliate, vedere [versione di valutazione gratuita di Windows Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

### <a name="create-a-web-site-and-a-sql-database-in-windows-azure"></a>Creare un sito web e un database SQL in Azure

Il sito Web di Microsoft Azure verrà eseguito in un ambiente di hosting condiviso, ovvero che viene eseguito in macchine virtuali (VM) che vengono condivisi con altri client di Windows Azure. Un ambiente di hosting condiviso è un modo economico per iniziare a usare nel cloud. In un secondo momento, se l'incremento del traffico web, la scalabilità per soddisfare le esigenze tramite l'esecuzione in macchine virtuali dedicate. Se è necessaria un'architettura più complessa, è possibile eseguire la migrazione a un servizio Cloud di Azure. Servizi cloud vengono eseguiti in macchine virtuali dedicate che è possibile configurare in base alle esigenze.

Database SQL di Azure è un servizio di database relazionale basato su cloud che si basa su tecnologie SQL Server. Strumenti e le applicazioni che funzionano con SQL Server sono utilizzabili anche con il Database SQL.

1. Nel [portale di gestione di Microsoft Azure](https://manage.windowsazure.com/), fare clic su **siti Web** nella scheda a sinistra e quindi fare clic su **New**.

    ![Pulsante nuovo nel portale di gestione](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)
2. Fare clic su **creazione personalizzata**.

    ![Crea con Database collegamento nel portale di gestione](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

   Il **nuovo sito Web - creazione personalizzata** verrà visualizzata la procedura guidata.
3. Nel **nuovo sito Web** passaggio della procedura guidata, immettere una stringa nel **URL** finestra da usare come URL univoco per l'applicazione. L'URL completo sarà costituito da quanto immesso e dal suffisso visualizzato accanto alla casella di testo. La figura mostra "ConU", ma tale URL è probabilmente già occupato, pertanto è necessario sceglierne uno diverso.

    ![Crea con Database collegamento nel portale di gestione](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)
4. Nel **regione** elenco a discesa scegliere un'area vicina. Questa impostazione specifica il sito web verrà eseguito in quale data center.
5. Nel **Database** elenco a discesa scegliere **crea un database SQL 20 MB gratuito**.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)
6. Nel **DB CONNECTION STRING NAME**, immettere *SchoolContext*.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)
7. Fare clic sulla freccia che punta a destra nella parte inferiore della finestra. Verrà visualizzata la **le impostazioni del Database** passaggio.
8. Nel **Name** casella, immettere *ContosoUniversityDB*.
9. Nel **Server** , quindi selezionare **server di Database SQL nuova**. In alternativa, se è stato creato un server, è possibile selezionare quel server dall'elenco a discesa.
10. Immettere un amministratore **nome account di accesso** e **PASSWORD**. Se è stato selezionato **server di Database SQL nuova** non si immetteranno un nome esistente e una password, immettere un nuovo nome e una password che si sta definendo ora da usare in seguito quando si accede al database. Se è stato selezionato un server creato in precedenza, immettere le credenziali per il server. Per questa esercitazione, non verrà scelto il ***avanzate*** casella di controllo. Il ***avanzate*** le opzioni consentono di impostare il database [regole di confronto](https://msdn.microsoft.com/library/aa174903(v=SQL.80).aspx).
11. Scegliere lo stesso **regione** scelto per il sito web.
12. Scegliere il segno di spunta nella parte inferiore destra della casella per indicare che si è finito.   
  
    ![Passaggio di impostazioni di database del nuovo sito di Web - Create con Creazione guidata Database](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image16.png)  

    L'immagine seguente viene illustrato l'utilizzo di un Server SQL esistente e un account di accesso.   
  
    ![Passaggio di impostazioni di database del nuovo sito di Web - Create con Creazione guidata Database](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image17.png)  
  
    Il portale di gestione restituisce alla pagina di siti Web e il **stato** colonna indica che il sito viene creato. Dopo un periodo di tempo (in genere meno di un minuto), il **stato** colonna mostra che il sito è stato creato correttamente. Nella barra di spostamento a sinistra, il numero di siti presenti nell'account viene visualizzato accanto al **siti Web** icona e il numero di database viene visualizzato accanto al **database SQL** icona.

## <a name="deploy-the-application-to-windows-azure"></a>Distribuire l'applicazione in Windows Azure

1. In Visual Studio, fare clic sul progetto in **Esplora soluzioni** e selezionare **Publish** dal menu di scelta rapida.  
  
    ![Pubblicare nel menu di scelta rapida progetto](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image18.png)
2. Nel **profilo** scheda della finestra di **Pubblica sito Web** procedura guidata, fare clic su **importazione**.  
  
    ![Importa impostazioni di pubblicazione](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image19.png)
3. Se non è stato aggiunto in precedenza la sottoscrizione Windows Azure in Visual Studio, eseguire la procedura seguente. In questi passaggi aggiungere la sottoscrizione in modo che nell'elenco a discesa elenco **Importa da un sito web di Microsoft Azure** includerà il sito web.

    a. Nel **Importa profilo di pubblicazione** finestra di dialogo, fare clic su **Importa da un sito web di Azure**e quindi fare clic su **sottoscrizione aggiungere Windows Azure**.

    ![Aggiungi sottoscrizione Windows Azure](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image20.png)

    b. Nel **Importa sottoscrizioni di Azure di Windows** finestra di dialogo, fare clic su **Scarica file di sottoscrizione**.

    ![scaricare il file di sottoscrizione](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image21.png)

    c. Nella finestra del browser, Salva il *publishsettings* file.

    ![scaricare il file con estensione publishsettings](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image22.png)

    > [!WARNING]
    > Security - i *publishsettings* file contiene le credenziali (non codificate) usate per amministrare i servizi e le sottoscrizioni Windows Azure. La procedura consigliata di sicurezza per questo file consiste nell'archiviarlo temporaneamente all'esterno delle directory di origine (ad esempio nel *raccolte\documenti* cartella) e quindi eliminarlo al termine dell'importazione è stata completata. Un utente malintenzionato che riesce ad accedere al `.publishsettings` file può modificare, creare ed eliminare i servizi di Azure.

    d. Nel **Importa sottoscrizioni di Azure di Windows** della finestra di dialogo fare clic su **Sfoglia** e passare al *publishsettings* file.

    ![scaricare sub](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image23.png)

    e. Fare clic su **Importa**.

    ![importazione](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image24.png)
4. Nel **Importa profilo di pubblicazione** finestra di dialogo **Importa dati da un sito web di Azure**, selezionare il sito web nell'elenco a discesa e quindi fare clic su **OK**.  
  
    ![Importazione profilo di pubblicazione](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image25.png)
5. Nel **Connection** scheda, fare clic su **convalida connessione** per assicurarsi che le impostazioni siano corrette.  
  
    ![Convalidare la connessione](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)
6. Quando la connessione è stata convalidata, un segno di spunta verde verrà visualizzato accanto al **convalida connessione** pulsante. Scegliere **Avanti**.  
  
    ![Connessione è stata convalidata](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image27.png)
7. Aprire il **stringa di connessione remota** elenco a discesa sotto **SchoolContext** e selezionare la stringa di connessione per il database è stato creato.
8. Selezionare **Esegui migrazioni Code First (inizia all'avvio dell'applicazione)**.
9. Deselezionare l'opzione **utilizzare questa stringa di connessione in fase di esecuzione** per il **UserContext (DefaultConnection)**, dal momento che questa applicazione non usa il database di appartenenza.   
  
    ![Scheda Impostazioni](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image28.png)
10. Scegliere **Avanti**.
11. Nel **Preview** scheda, fare clic su **Avvia anteprima**.  
  
    ![Pulsante StartPreview nella scheda Anteprima](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image29.png)  
  
    La scheda Visualizza un elenco dei file che verranno copiati nel server. Visualizzazione dell'anteprima per pubblicare l'applicazione non è necessario ma è una funzione utile da tenere presenti. In questo caso, non devi eseguire alcuna operazione con l'elenco di file che viene visualizzato. La volta successiva che si distribuirà questa applicazione, sarà solo i file che sono stati modificati in questo elenco.  
  
    ![Output del file StartPreview](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image30.png)
12. Fare clic su **Pubblica**.  
    Visual Studio avvia il processo di copia dei file nel server di Windows Azure.
13. Il **Output** finestra Mostra le azioni di distribuzione sono state eseguite e segnalato il corretto completamento della distribuzione.  
  
    ![Finestra di output di segnalazione della corretta distribuzione](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image31.png)
14. Al termine della distribuzione, il browser predefinito viene aperto automaticamente per l'URL del sito web distribuito.  
    L'applicazione creata è in esecuzione nel cloud. Fare clic sulla scheda Students.  
  
    ![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image32.png)

A questo punto il *SchoolContext* database è stato creato nel Database SQL di Azure di Windows perché è stato selezionato **Esegui migrazioni Code First (inizia all'avvio dell'app)**. Il *Web. config* file nel sito web distribuito è stato modificato in modo che le [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inizializzatore verrebbe eseguito la prima volta il codice legge o scrive dati nel database (che si è verificato quando è selezionata la **studenti** scheda):

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image33.png)

Il processo di distribuzione creata anche una nuova stringa di connessione *(SchoolContext\_DatabasePublish*) per le migrazioni Code First da utilizzare per l'aggiornamento dello schema del database e il seeding del database.

![Stringa di connessione Database_Publish](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image34.png)

Il *DefaultConnection* stringa di connessione è per il database di appartenenza (che non viene usato in questa esercitazione). Il *SchoolContext* stringa di connessione è per il database ContosoUniversity.

È possibile trovare la versione distribuita del file Web. config nel computer in *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. È possibile accedere a distribuito *Web. config* file tramite FTP. Per istruzioni, vedere [distribuzione Web ASP.NET tramite Visual Studio: distribuzione di un aggiornamento del codice](../../../../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Seguire le istruzioni che iniziano con "per usare uno strumento FTP, sono necessarie tre cose: l'URL di FTP, il nome utente e la password."

> [!NOTE]
> L'app web non implementa la sicurezza, in modo che chiunque individui l'URL può modificare i dati. Per istruzioni su come proteggere il sito web, vedere [distribuire un'app ASP.NET MVC sicura con appartenenza, OAuth e Database SQL a un sito Web di Microsoft Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). È possibile impedire che altri utenti tramite il sito usando il portale di gestione di Azure oppure **Esplora Server** in Visual Studio per arrestare il sito.


![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image35.png)

## <a name="code-first-initializers"></a>Gli inizializzatori prima del codice

Nella sezione distribuzione è stato illustrato il [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inizializzatore in uso. Codice prima di tutto fornisce anche altri inizializzatori che è possibile utilizzare, compresi [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (impostazione predefinita), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) e [ DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). Il `DropCreateAlways` inizializzatore può essere utile per impostare le condizioni per gli unit test. È anche possibile scrivere i propri inizializzatori ed è possibile chiamare un inizializzatore in modo esplicito, se non si vuole attendere fino a quando l'applicazione legge o scrive nel database. Per una spiegazione completa degli inizializzatori, vedere il capitolo 6 del libro [Programming Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman e Rowan Miller.

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato spiegato come creare un modello di dati e implementare CRUD di base, l'ordinamento, filtro, paging e la funzionalità di raggruppamento. Nella prossima esercitazione verrà iniziare esaminando gli argomenti più avanzati espandendo il modello di dati.

Sono disponibili collegamenti ad altre risorse di Entity Framework nel [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Precedente](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [Successivo](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
