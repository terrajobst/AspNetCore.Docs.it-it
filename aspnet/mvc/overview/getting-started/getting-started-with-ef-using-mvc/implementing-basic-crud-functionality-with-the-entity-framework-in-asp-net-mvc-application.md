---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: 'Esercitazione: Implementare la funzionalità CRUD con Entity Framework in ASP.NET MVC | Microsoft Docs'
description: Esaminare e personalizzare la creazione, leggere, aggiornare ed eliminare code (CRUD) che lo scaffolding di MVC crea automaticamente nel controller e visualizzazioni.
author: tdykstra
ms.author: riande
ms.date: 01/11/2019
ms.topic: tutorial
ms.assetid: a2f70ba4-83d1-4002-9255-24732726c4f2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9c6f8f3a2ffc0a9c5e15111ae47c331dab24ff43
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341724"
---
# <a name="tutorial-implement-crud-functionality-with-the-entity-framework-in-aspnet-mvc"></a>Esercitazione: Implementare la funzionalità CRUD con Entity Framework in ASP.NET MVC

Nel [esercitazione precedente](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md), è stata creata un'applicazione MVC che memorizza e visualizza i dati utilizzando Entity Framework (EF) 6 e LocalDB di SQL Server. In questa esercitazione esaminare e personalizzare la creazione, leggere, aggiornare, eliminare il codice (CRUD) che lo scaffolding di MVC crea automaticamente per l'utente nel controller e visualizzazioni.

> [!NOTE]
> È pratica comune implementare il modello di repository per creare un livello di astrazione tra il controller e il livello di accesso ai dati. Per mantenere queste esercitazioni semplici e incentrate sulla descrizione di come usare Entity Framework 6 stesso, non usano i repository. Per informazioni su come implementare i repository, vedere la [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

Ecco alcuni esempi delle pagine web create:

![Screenshot della pagina dei dettagli degli studenti.](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Screenshot dello studente è creare una pagina.](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Screenshot dello studente ot Elimina pagina.](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Creare una pagina di dettagli
> * Aggiornare la pagina Create
> * Aggiornare il metodo di modifica HttpPost
> * Aggiornare la pagina Delete (Elimina)
> * Connessioni al database Chiudi
> * Gestione delle transazioni

## <a name="prerequisites"></a>Prerequisiti

* [Creare il modello di dati di Entity Framework](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

## <a name="create-a-details-page"></a>Creare una pagina di dettagli

Il codice con scaffolding per studenti `Index` pagina, esclusi il `Enrollments` proprietà, poiché la proprietà contiene una raccolta. Nel `Details` pagina verranno visualizzati il contenuto della raccolta in una tabella HTML.

Nelle *Controllers\StudentController.cs*, il metodo di azione per il `Details` visualizzare Usa il [trovare](https://msdn.microsoft.com/library/gg696418(v=VS.103).aspx) metodo per recuperare una singola `Student` entità.

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

Il valore della chiave viene passato al metodo come la `id` parametro e deriva da *instradare i dati* nel **dettagli** collegamento ipertestuale nella pagina di indice.

### <a name="tip-route-data"></a>Suggerimento: **Dati della route**

I dati di route sono che il gestore di associazione di modelli disponibili in un segmento di URL specificato nella tabella di routing. Ad esempio, la route predefinita specifica `controller`, `action`, e `id` segmenti:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cs?highlight=3)]

L'URL seguente, la route predefinita mappa `Instructor` come il `controller`, `Index` come il `action` e 1 come il `id`; questi sono i valori dei dati di route.

`http://localhost:1230/Instructor/Index/1?courseID=2021`

`?courseID=2021` è un valore di stringa di query. Lo strumento individuerebbe funzionerà anche se si passa il `id` come un valore di stringa di query:

`http://localhost:1230/Instructor/Index?id=1&CourseID=2021`

Gli URL creati da `ActionLink` istruzioni nella visualizzazione Razor. Nel codice seguente, il `id` parametro corrisponde alla route predefinita, pertanto `id` viene aggiunto ai dati della route.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml)]

Nel codice seguente, `courseID` non corrisponde a un parametro della route predefinita, viene aggiunto come una stringa di query.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cshtml)]

### <a name="to-create-the-details-page"></a>Per creare la pagina dei dettagli

1. Aprire *Views\Student\Details.cshtml*.

   Ogni campo viene visualizzato utilizzando un `DisplayFor` helper, come illustrato nell'esempio seguente:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cshtml)]

2. Dopo il `EnrollmentDate` campo e immediatamente prima della chiusura `</dl>` tag, aggiungere il codice evidenziato per visualizzare un elenco delle registrazioni, come illustrato nell'esempio seguente:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml?highlight=8-29)]

    Se dopo aver incollato il codice, rientro è errato, premere **Ctrl**+**K**, **Ctrl**+**1!d** da formattare si tratta.

    Il codice esegue il ciclo nelle entità nella proprietà di navigazione `Enrollments`. Per ogni `Enrollment` entità nella proprietà, viene visualizzato il titolo del corso e il livello. Il titolo del corso viene recuperato dal `Course` entità a cui viene archiviato nel `Course` proprietà di navigazione del `Enrollments` entità. Tutti i dati vengono recuperati dal database automaticamente quando è necessaria. In altre parole, si usa il caricamento lazy qui. Non è stato specificato *caricamento eager* per il `Courses` proprietà di navigazione, in modo che le registrazioni non sono state recuperate nella stessa query è stata ricevuta gli studenti. Al contrario, la prima volta che si tenta di accedere il `Enrollments` proprietà di navigazione, una nuova query viene inviata al database per recuperare i dati. Altre informazioni sulle modalità di caricamento lazy e il caricamento eager nel [lettura di dati correlati](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) esercitazione più avanti in questa serie.

3. Aprire la pagina dei dettagli avviando il programma (**Ctrl**+**F5**), se si seleziona il **studenti** scheda e quindi scegliendo il **dettagli** collegamento per Alexander Carson. (Se si preme **Ctrl**+**F5** mentre il *Details. cshtml* file è aperto, si verifica un errore HTTP 400. Questo avviene perché Visual Studio tenta di eseguire la pagina dei dettagli, ma non è stato raggiunto da un collegamento che specifica lo studente da visualizzare. Se in tal caso, rimuovere "Student/dettagli" dall'URL e ripetere l'operazione, in alternativa, chiudere il browser, fare clic sul progetto e fare clic su **View** > **Visualizza nel Browser**.)

    Viene visualizzato l'elenco dei corsi e dei voti dello studente selezionato.

4. Chiudere il browser.

## <a name="update-the-create-page"></a>Aggiornare la pagina Create

1. Nelle *Controllers\StudentController.cs*, sostituire il <xref:System.Web.Mvc.HttpPostAttribute> `Create` metodo di azione con il codice seguente. Questo codice aggiunge un `try-catch` blocco e rimuove `ID` dal <xref:System.Web.Mvc.BindAttribute> attributo per il metodo sottoposto a scaffolding:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=3,5-6,13-18)]

    Questo codice aggiunge il `Student` entità creata dallo strumento di associazione di modelli ASP.NET MVC per la `Students` entità impostata e quindi Salva le modifiche apportate al database. *Strumento di associazione del modello* fa riferimento a funzionalità di ASP.NET MVC che rende più semplice per lavorare con i dati inviati da un modulo; uno strumento di associazione di modello modulo converte registrati i valori per i tipi CLR e li passa al metodo di azione nei parametri. In questo caso, lo strumento di associazione di modello crea un'istanza una `Student` entità con proprietà i valori di `Form` raccolta.

    È stato rimosso `ID` dall'associazione attributo perché `ID` corrisponde al valore di chiave primario che SQL Server verrà impostato automaticamente quando viene inserita la riga. Input dell'utente, non viene impostata la `ID` valore.

    <a id="overpost"></a>

    ### <a name="security-warning---the-validateantiforgerytoken-attribute-helps-prevent-cross-site-request-forgerysecurityxsrfcsrf-prevention-in-aspnet-mvc-and-web-pagesmd-attacks-it-requires-a-corresponding-htmlantiforgerytoken-statement-in-the-view-which-youll-see-later"></a>Avviso di sicurezza - il `ValidateAntiForgeryToken` attributo consente di impedire [XSS richiesta intersito falsa](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) attacchi. Richiede un oggetto corrispondente `Html.AntiForgeryToken()` istruzione nella vista, che verrà visualizzato in un secondo momento.

    Il `Bind` attributo è un modo per proteggersi contro *overposting* negli scenari di creazione. Ad esempio, si supponga che il `Student` entità include un `Secret` proprietà che non si desidera questa pagina web da impostare.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs?highlight=7)]

    Anche se non hai un `Secret` campo nella pagina web, un hacker potrebbe usare uno strumento, ad esempio [fiddler](http://fiddler2.com/home), oppure scrivere codice JavaScript, per registrare un `Secret` valore di modulo. Senza il <xref:System.Web.Mvc.BindAttribute> attributo limita i campi che lo strumento di associazione di modelli Usa durante la creazione di un `Student` istanza<em>,</em> lo strumento individuerebbe preleverebbe che `Secret` valore di modulo e usarlo per creare il `Student` istanza di entità. Di conseguenza, qualsiasi valore specificato dall'hacker per il campo di modulo `Secret` verrebbe aggiornato nel database. L'immagine seguente mostra il fiddler degli strumenti aggiungendo il `Secret` campo (con il valore "OverPost") per i valori di modulo.

    ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

    Il valore "OverPost" verrebbe quindi aggiunto alla proprietà `Secret` della riga inserita, sebbene non sia prevista in alcun modo l'impostazione della proprietà da parte della pagina Web.

    È consigliabile usare la `Include` parametro con il `Bind` dell'attributo *whitelist* campi. È anche possibile usare la `Exclude` parametro per *blacklist* campi che si desidera escludere. Il motivo `Include` è più sicuro è che, quando si aggiunge una nuova proprietà per l'entità, il nuovo campo non automaticamente protetti da un `Exclude` elenco.

    È possibile impedire l'overposting negli scenari di modifica è la lettura di entità dal database prima di tutto e chiamando quindi `TryUpdateModel`, passando un elenco delle proprietà consentite esplicito. Si tratta del metodo usato in queste esercitazioni.

    Un modo alternativo per impedire l'overposting numerosi sviluppatori è usare i modelli di visualizzazione anziché le classi di entità con associazione di modelli. Includere solo le proprietà da aggiornare nel modello di visualizzazione. Al termine lo strumento di associazione di modelli MVC, copiare le proprietà del modello di visualizzazione per l'istanza dell'entità usando facoltativamente uno strumento, ad esempio [AutoMapper](http://automapper.org/). Usare db. Voce nell'istanza dell'entità per impostarne lo stato su Unchanged e quindi impostare Property("PropertyName"). IsModified su true in ogni proprietà dell'entità è incluso nel modello di visualizzazione. Questo metodo funziona negli scenari di modifica e negli scenari di creazione.

    Diverso dal `Bind` attributo, il `try-catch` blocco è l'unica modifica apportate al codice con scaffolding. Se viene rilevata un'eccezione che deriva da <xref:System.Data.DataException> durante il salvataggio delle modifiche, viene visualizzato un messaggio di errore generico. Poiché le eccezioni <xref:System.Data.DataException> sono a volte causate da elementi esterni all'applicazione e non da un errore di programmazione, viene consigliato all'utente di riprovare. Sebbene non sia implementata in questo esempio, un'applicazione di controllo della qualità di produzione potrebbe registrare l'eccezione. Per altre informazioni, vedere la sezione **Log for insight** (Registrare informazioni dettagliate) in [Monitoring and Telemetry (Building Real-World Cloud Apps with Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) (Monitoraggio e telemetria (creazione di app cloud realistiche con Azure)).

    Il codice nel *Views\Student\Create.cshtml* è simile a quello illustrato *Details. cshtml*, ad eccezione del fatto che `EditorFor` e `ValidationMessageFor` gli helper vengono usati per ogni campo anziché `DisplayFor`. Ecco il codice pertinente:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cshtml)]

    *Create.chstml* include inoltre `@Html.AntiForgeryToken()`, che funziona con i `ValidateAntiForgeryToken` attributo nel controller al fine di evitare [intersito richiesta intersito falsa](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) attacchi.

    Non sono necessarie modifiche nel *create. cshtml*.

2. Eseguire la pagina avviando il programma, selezionando il **studenti** scheda e quindi scegliendo **Crea nuovo**.

3. Immettere i nomi e una data non valida e fare clic su **Create** per visualizzare il messaggio di errore.

    Si tratta di convalida sul lato server che si ottiene per impostazione predefinita. In un'esercitazione successiva, verrà illustrato come aggiungere gli attributi che generano codice per la convalida lato client. Il codice evidenziato seguente illustra il controllo di convalida del modello nel **Create** (metodo).

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs?highlight=1)]

4. Modificare la data impostando un valore valido e fare clic su **Crea** per visualizzare il nuovo studente nella pagina **Index**.

5. Chiudere il browser.

## <a name="update-httppost-edit-method"></a>Aggiornare il metodo di modifica HttpPost

1. Sostituire il <xref:System.Web.Mvc.HttpPostAttribute> `Edit` metodo di azione con il codice seguente:

   [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

   > [!NOTE]
   > Nel *Controllers\StudentController.cs*, il `HttpGet Edit` (metodo) (quello senza il `HttpPost` attributo) usa il `Find` metodo per recuperare l'oggetto selezionato `Student` entità, come illustrato nella `Details`metodo. Non è necessario modificare questo metodo.

   Queste modifiche implementano una protezione ottimale per impedire [overposting](#overpost), lo scaffolder ha generato un `Bind` attributo e aggiungere l'entità creata dal binder di modello per il set di entità con un flag di modifica. Che codice non è più consigliato perché il `Bind` attributo Cancella tutti i dati esistenti nei campi non elencati nel `Include` parametro. In futuro, l'utilità di scaffolding di controller MVC verrà aggiornato in modo che non generi `Bind` attributi per i metodi di modifica.

   Il nuovo codice legge l'entità esistente e le chiamate <xref:System.Web.Mvc.Controller.TryUpdateModel%2A> per aggiornare i campi dall'input dell'utente nei dati di modulo inviati. Rilevamento set delle modifiche automatico di Entity Framework la [EntityState.Modified](<xref:System.Data.EntityState.Modified>) flag nell'entità. Quando la [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) viene chiamato il metodo di <xref:System.Data.EntityState.Modified> flag fa in modo che Entity Framework creare istruzioni SQL per aggiornare la riga del database. [I conflitti di concorrenza](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) vengono ignorati e vengono aggiornate tutte le colonne della riga di database, inclusi quelli che l'utente non è stato modificato. (Un'esercitazione successiva illustra come gestire i conflitti di concorrenza e se si vuole solo i singoli campi da aggiornare nel database, è possibile impostare l'entità su [EntityState.Unchanged](<xref:System.Data.EntityState.Unchanged>) e impostare i singoli campi su [ EntityState.Modified](<xref:System.Data.EntityState.Modified>).)

   Per impedire l'overposting, i campi che si desidera essere aggiornati dalla pagina di modifica sono consentiti nel `TryUpdateModel` parametri. Sebbene attualmente non siano presenti campi aggiuntivi da proteggere, la creazione di un elenco dei campi che devono essere associati dallo strumento di associazione di modelli garantisce che eventuali campi aggiunti in seguito al modello di dati vengano protetti automaticamente fino a quando non vengono aggiunti in modo esplicito in questa posizione.

   In seguito a queste modifiche, la firma del metodo del metodo HttpPost Edit è quello utilizzato dal metodo edit HttpGet; di conseguenza è stato rinominato il metodo EditPost.

   > [!TIP]
   >
   > **Stati di entità e le collega e i metodi SaveChanges**
   >
   > Il contesto del database rileva se le entità in memoria sono sincronizzate con le righe corrispondenti nel database e queste informazioni determinano le operazioni eseguite quando viene chiamato il metodo `SaveChanges`. Ad esempio, quando si passa una nuova entità per il [Add](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) metodo, lo stato dell'entità viene impostato su `Added`. Quando si chiama il [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metodo, il contesto del database esegue un database SQL `INSERT` comando.
   >
   > Le entità possono essere in uno dei seguenti [stati](xref:System.Data.EntityState):
   >
   > - `Added`. L'entità non esiste ancora nel database. Il `SaveChanges` metodo deve generare un `INSERT` istruzione.
   > - `Unchanged`. Il metodo `SaveChanges` non deve eseguire alcuna operazione con l'entità. Quando un'entità viene letta dal database, l'entità ha inizialmente questo stato.
   > - `Modified`. Sono stati modificati alcuni o tutti i valori di proprietà dell'entità. Il `SaveChanges` metodo deve generare un `UPDATE` istruzione.
   > - `Deleted`. L'entità è stata contrassegnata per l'eliminazione. Il `SaveChanges` metodo deve generare un `DELETE` istruzione.
   > - `Detached`. L'entità non viene registrata dal contesto del database.
   >
   > In un'applicazione desktop le modifiche dello stato vengono in genere impostate automaticamente. In un tipo desktop dell'applicazione, viene letta un'entità e apportare modifiche ad alcuni dei valori delle proprietà. In questo modo lo stato dell'entità viene modificato automaticamente in `Modified`. Quando si chiama `SaveChanges`, Entity Framework genera un codice SQL `UPDATE` istruzione che aggiorna solo le proprietà che è stato modificato.
   >
   > Per questa sequenza continua non consente la natura disconnessa delle App web. Il [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) che legge un'entità viene eliminata dopo il rendering di una pagina. Quando la `HttpPost` `Edit` viene chiamato il metodo di azione, viene effettuata una nuova richiesta e si dispone di una nuova istanza della [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), pertanto è necessario impostare manualmente lo stato dell'entità `Modified.` quindi quando si chiama `SaveChanges`, Entity Framework Aggiorna tutte le colonne della riga di database, poiché il contesto non ha modo di conoscere le proprietà modificate.
   >
   > Se si desidera che il codice SQL `Update` istruzione per aggiornare solo i campi modificati dall'utente, è possibile salvare i valori originali in qualche modo (ad esempio campi nascosti) in modo che siano disponibili quando il `HttpPost` `Edit` viene chiamato il metodo. È possibile creare un `Student` entità usando i valori originali, chiamare il `Attach` metodo con tale versione originale dell'entità, aggiornare i valori dell'entità per i nuovi valori e quindi chiamare `SaveChanges.` per altre informazioni, vedere [ Stati di entità e SaveChanges](/ef/ef6/saving/change-tracking/entity-state) e [i dati locali](/ef/ef6/querying/local-data).

   Il codice HTML e Razor *Views\Student\Edit.cshtml* è simile a quello illustrato *create. cshtml*, e non sono necessarie modifiche.

2. Eseguire la pagina avviando il programma, selezionando il **studenti** scheda e quindi facendo clic su un **modificare** collegamento ipertestuale.

3. Modificare alcuni dati e fare clic su **Salva**. Noterete che i dati modificati nella pagina di indice.

4. Chiudere il browser.

## <a name="update-the-delete-page"></a>Aggiornare la pagina Delete (Elimina)

In *Controllers\StudentController.cs*, il codice del modello per il <xref:System.Web.Mvc.HttpGetAttribute> `Delete` metodo Usa il `Find` metodo per recuperare l'oggetto selezionato `Student` entità, come illustrato nella `Details` e `Edit` metodi. Tuttavia, per implementare un messaggio di errore personalizzato quando la chiamata di `SaveChanges` ha esito negativo, verrà aggiunta una funzionalità al metodo e alla visualizzazione corrispondente.

Analogamente alle operazioni di aggiornamento e creazione, le operazioni di eliminazione richiedono due metodi di azione. Il metodo che viene chiamato in risposta a una richiesta GET apre una visualizzazione che consente all'utente di approvare o annullare l'operazione di eliminazione. Se l'utente approva, viene creata una richiesta POST. In questo caso, il `HttpPost` `Delete` viene chiamato il metodo e quindi tale metodo esegue effettivamente l'operazione di eliminazione.

Si aggiungerà un `try-catch` bloccare per il <xref:System.Web.Mvc.HttpPostAttribute> `Delete` metodo per gestire eventuali errori che potrebbero verificarsi quando il database viene aggiornato. Se si verifica un errore, il <xref:System.Web.Mvc.HttpPostAttribute> `Delete` chiamate al metodo il <xref:System.Web.Mvc.HttpGetAttribute> `Delete` metodo, passando un parametro che indica che si è verificato un errore. Il <xref:System.Web.Mvc.HttpGetAttribute> `Delete` metodo visualizza nuovamente la pagina di conferma con il messaggio di errore offrendo all'utente la possibilità di annullare o ripetere l'operazione.

1. Sostituire il <xref:System.Web.Mvc.HttpGetAttribute> `Delete` metodo di azione con il codice seguente, che gestisce la segnalazione errori:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cs?highlight=1,7-10)]

    Questo codice accetta un [parametro facoltativo](https://msdn.microsoft.com/library/dd264739.aspx) che indica se il metodo è stato chiamato dopo un errore durante il salvataggio delle modifiche. Questo parametro è `false` quando il `HttpGet` `Delete` viene chiamato senza un precedente errore. Quando viene chiamato il `HttpPost` `Delete` metodo in risposta a un errore di aggiornamento del database, il parametro è `true` e un messaggio di errore viene passato alla visualizzazione.

2. Sostituire il <xref:System.Web.Mvc.HttpPostAttribute> `Delete` metodo di azione (denominato `DeleteConfirmed`) con il codice seguente, che esegue l'operazione di eliminazione e rileva eventuali errori di aggiornamento del database.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

    Questo codice recupera l'entità selezionata, quindi chiama il [rimuovere](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) metodo per impostare lo stato dell'entità su `Deleted`. Quando `SaveChanges` viene chiamato, un database SQL `DELETE` comando viene generato. Anche il nome del metodo di azione è stato modificato da `DeleteConfirmed` a `Delete`. Il nome in codice con scaffolding il `HttpPost` `Delete` metodo `DeleteConfirmed` per fornire la `HttpPost` metodo una firma univoca. (Common Language Runtime richiede che i metodi di overload dispongano di parametri di metodo diversi). Ora che le firme sono univoche, è possibile utilizzare la convenzione di MVC e usare lo stesso nome per il `HttpPost` e `HttpGet` metodi di eliminazione.

    Se il miglioramento delle prestazioni in un'applicazione di grandi volumi è una priorità, è possibile evitare una query SQL non necessaria per recuperare la riga da sostituire le righe di codice che chiamano il `Find` e `Remove` metodi con il codice seguente:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample14.cs)]

    Questo codice crea un `Student` entità usando solo il valore di chiave primaria e quindi imposta lo stato dell'entità `Deleted`. Questo è tutto ciò di cui Entity Framework necessita per eliminare l'entità.

    Come accennato, il `HttpGet` `Delete` metodo non comporta l'eliminazione dei dati. Esecuzione di un'operazione di eliminazione in risposta a una richiesta GET request (o per l'esecuzione di qualsiasi operazione di modifica, creazione o qualsiasi altra operazione che modifica i dati) crea un rischio per la sicurezza. Per altre informazioni, vedere [46 & suggerimento di ASP.NET MVC, non usare i collegamenti di eliminazione poiché creano le falle](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) sul blog di Stephen Walther.

3. Nelle *Views\Student\Delete.cshtml*, aggiungere un messaggio di errore tra il `h2` intestazione e il `h3` intestazione, come illustrato nell'esempio seguente:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample15.cshtml?highlight=2)]

4. Eseguire la pagina avviando il programma, selezionando il **studenti** scheda e quindi facendo clic su un **eliminare** collegamento ipertestuale.

5. Scegli **eliminare** nella pagina con la dicitura **si è sicuri di voler eliminare questo?**.

    Consente di visualizzare la pagina Index senza lo studente eliminato. (È disponibile un esempio del codice in azione nella gestione degli errori di [esercitazione sulla concorrenza](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md).)

## <a name="close-database-connections"></a>Connessioni al database Chiudi

Per chiudere le connessioni al database e liberare le risorse che contengono appena possibile, eliminare l'istanza del contesto dopo che aver con esso. Vale a dire il motivo per cui il codice con scaffolding include un [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) alla fine del metodo le `StudentController` classe *StudentController.cs*, come illustrato nell'esempio seguente:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample16.cs)]

La base `Controller` classe già implementa le `IDisposable` interfaccia, in modo che questo codice aggiunge semplicemente una sostituzione per il `Dispose(bool)` metodo per eliminare in modo esplicito l'istanza del contesto.

## <a name="handle-transactions"></a>Gestione delle transazioni

Per impostazione predefinita Entity Framework implementa in modo implicito le transazioni. Negli scenari in cui si apportano modifiche a più righe o le tabelle e quindi chiamare `SaveChanges`, Entity Framework verifica automaticamente che che tutte le modifiche abbia esito positivo o non è nessuna. Se sono state apportate alcune modifiche e successivamente si verifica un errore, viene automaticamente eseguito il rollback di tali verifiche. Per gli scenari in cui è necessario un maggior controllo&mdash;ad esempio, se si desidera includere le operazioni eseguite all'esterno di Entity Framework in una transazione&mdash;vedere [utilizzo di transazioni](/ef/ef6/saving/transactions).

## <a name="additional-resources"></a>Risorse aggiuntive

Ora è un set completo di pagine che eseguono operazioni CRUD semplici per `Student` entità. Helper di MVC è utilizzato per generare gli elementi dell'interfaccia utente per i campi dati. Per altre informazioni sull'helper di MVC, vedere [il Rendering di un helper HTML usando Form](/previous-versions/aspnet/dd410596(v=vs.98)) (l'articolo è per MVC 3, ma sono ancora rilevanti per MVC 5).

Collegamenti ad altre risorse di Entity Framework 6 sono reperibile nel [l'accesso ai dati ASP.NET - risorse consigliate](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Passaggi successivi

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Creazione di una pagina di dettagli
> * Aggiornata la pagina Create
> * Aggiornato il metodo di modifica HttpPost
> * Aggiornata la pagina Delete
> * Connessioni al database chiusa
> * Transazioni gestite

Passare all'articolo successivo per informazioni su come aggiungere l'ordinamento, filtro e paging per il progetto.
> [!div class="nextstepaction"]
> [Ordinamento, filtro e paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)