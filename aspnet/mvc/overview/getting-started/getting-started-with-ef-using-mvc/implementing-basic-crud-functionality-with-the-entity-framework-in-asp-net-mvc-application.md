---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: "Implementazione della funzionalità CRUD di base con Entity Framework nell'applicazione ASP.NET MVC | Documenti Microsoft"
author: tdykstra
description: L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 5 con Code First di Entity Framework 6 e Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/09/2015
ms.topic: article
ms.assetid: a2f70ba4-83d1-4002-9255-24732726c4f2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: c63b8f591023b68720c523d1c9184a527a34e9cc
ms.sourcegitcommit: e4fb6b13be56a0fb2f2778623740a047d6489227
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/16/2017
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application"></a>Implementazione della funzionalità CRUD di base con Entity Framework in applicazione MVC ASP.NET
====================
Da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) o [Scarica il PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 5 con Visual Studio 2013 e Code First di Entity Framework 6. Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Nell'esercitazione precedente è stato creato un'applicazione MVC che archivia e Visualizza dati tramite Entity Framework sia LocalDB di SQL Server. In questa esercitazione verrà esaminare e personalizzare il CRUD (create, leggere, aggiornare ed eliminare) il codice che lo scaffolding di MVC il viene creata automaticamente nel controller e visualizzazioni.

> [!NOTE]
> È pratica comune per implementare il modello di repository per creare un livello di astrazione tra il controller e il livello di accesso ai dati. Per mantenere queste esercitazioni semplice e sono specifici per illustrare come utilizzare Entity Framework stesso, non usano repository. Per informazioni su come implementare repository, vedere il [mappa del contenuto ASP.NET dati accesso](../../../../whitepapers/aspnet-data-access-content-map.md).


In questa esercitazione si creerà le pagine web seguenti:

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

## <a name="create-a-details-page"></a>Creare una pagina di dettagli

Il codice di supporto temporaneo per studenti `Index` pagina tralasciato il `Enrollments` proprietà, in quanto tale proprietà contiene una raccolta. Nel `Details` pagina verranno visualizzati il contenuto della raccolta in una tabella HTML.

 In *Controllers\StudentController.cs*, il metodo di azione per il `Details` visualizzare utilizza il [trovare](https://msdn.microsoft.com/en-us/library/gg696418(v=VS.103).aspx) metodo per recuperare un singolo `Student` entità. 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

Il valore della chiave viene passato al metodo come la `id` parametro e provengono da *indirizzare dati* nel **dettagli** collegamento ipertestuale nella pagina di indice.

### <a name="tip-route-data"></a>Suggerimento: **indirizzare dati**

Dati di route sono presenti il gestore di associazione del modello in un segmento di URL specificato nella tabella di routing. Ad esempio, la route predefinita specifica `controller`, `action`, e `id` segmenti:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cs?highlight=3)]

L'URL seguente, la route predefinita viene eseguito il mapping `Instructor` come il `controller`, `Index` come il `action` e 1 come il `id`; si tratta di valori di dati di route.

`http://localhost:1230/Instructor/Index/1?courseID=2021`

"? courseID = 2021" è un valore di stringa di query. Lo strumento di associazione del modello funziona anche se si passa il `id` come valore di stringa di query:

`http://localhost:1230/Instructor/Index?id=1&CourseID=2021`

Vengono creati gli URL `ActionLink` istruzioni nella visualizzazione Razor. Nel codice seguente, il `id` parametro corrisponde la route predefinita, pertanto `id` viene aggiunto ai dati di route.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml)]

Nel codice seguente, `courseID` non corrisponde a un parametro di route predefinita, pertanto viene aggiunta come una stringa di query.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cshtml)]


1. Aprire *Views\Student\Details.cshtml*. Ogni campo viene visualizzato utilizzando un `DisplayFor` helper, come illustrato nell'esempio seguente:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cshtml)]
2. Dopo il `EnrollmentDate` e immediatamente prima della chiusura `</dl>` tag, aggiungere il codice evidenziato di seguito per visualizzare un elenco delle registrazioni, come illustrato nell'esempio seguente:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml?highlight=8-29)]

    Se dopo avere incollato il codice, rientro del codice non è corretto, premere CTRL-K-D per correggere l'errore.

    Questo codice consente di scorrere le entità di `Enrollments` proprietà di navigazione. Per ogni `Enrollment` entità nella proprietà, viene visualizzato il titolo del corso e il livello. Il titolo del corso viene recuperato dal `Course` entità che viene archiviato nel `Course` proprietà di navigazione del `Enrollments` entità. Tutti i dati vengono recuperati dal database automaticamente quando necessario. (In altre parole, si utilizza il caricamento lazy qui. Non è stato specificato *caricamento eager* per il `Courses` proprietà di navigazione, pertanto le registrazioni dei non sono state recuperate nella stessa query che ha ricevuto gli studenti. Al contrario, la prima volta che si tenta di accedere il `Enrollments` proprietà di navigazione, una nuova query viene inviata al database per recuperare i dati. Altre informazioni sul caricamento lazy e caricamento eager nel [lettura di dati correlati](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) esercitazione più avanti in questa serie.)
3. Eseguire la pagina selezionando il **studenti** scheda e facendo clic su un **dettagli** collegamento per Alexander Carson. (Se si preme CTRL + F5, mentre il file Details.cshtml è aperto, si otterrà un errore HTTP 400 perché Visual Studio tenta di eseguire la pagina dei dettagli, ma non è stato raggiunto da un collegamento che specifica gli studenti da visualizzare. Assume, semplicemente rimuovere "Student/dettagli" dall'URL e riprovare o chiudere il browser, fare clic sul progetto e fare clic su **vista**, quindi fare clic su **Visualizza nel Browser**.)

    Per gli studenti selezionato vedere l'elenco di corsi e livelli:

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="update-the-create-page"></a>Aggiornare la pagina di creazione

1. In *Controllers\StudentController.cs*, sostituire il `HttpPost``Create` metodo di azione con il codice seguente per aggiungere un `try-catch` blocco e rimuovere `ID` dal [attributo Bind](https://msdn.microsoft.com/en-us/library/system.web.mvc.bindattribute(v=vs.108).aspx) per il metodo di supporto temporaneo:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=3,5-6,13-18)]

    Questo codice aggiunge il `Student` entità creata dal gestore di associazione di modelli ASP.NET MVC per il `Students` set di entità e quindi Salva le modifiche apportate al database. (*Dello strumento di associazione del modello* fa riferimento alle funzionalità ASP.NET MVC rende più semplice da utilizzare con i dati inviati da un modulo; un raccoglitore di modelli di modulo registrati converte i valori ai tipi CLR e li passa al metodo di azione nei parametri. In questo caso, lo strumento di associazione crea un `Student` entità per la proprietà utilizzando i valori di `Form` insieme.)

    Si è rimosso `ID` da associare attributo perché `ID` è il valore di chiave primaria che SQL Server verrà impostato automaticamente quando viene inserita la riga. Input dell'utente non viene impostato il `ID` valore.

    <a id="overpost"></a>

    ### <a name="security-warning---the-validateantiforgerytoken-attribute-helps-prevent-cross-site-request-forgerysecurityxsrfcsrf-prevention-in-aspnet-mvc-and-web-pagesmd-attacks-it-requires-a-corresponding-htmlantiforgerytoken-statement-in-the-view-which-youll-see-later"></a>Avviso di sicurezza - il `ValidateAntiForgeryToken` attributo consente di evitare [intersito richieste false](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) attacchi. Richiede un corrispondente `Html.AntiForgeryToken()` istruzione nella vista, che verrà visualizzato in un secondo momento.

    Il `Bind` attributo è un modo per proteggersi da *registrazione eccessiva* nella creazione di scenari. Si supponga, ad esempio, il `Student` entità include un `Secret` proprietà che non si desidera questa pagina web da impostare.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs?highlight=7)]

    Anche se non è un `Secret` campo della pagina web, un pirata informatico potrebbe utilizzare uno strumento come [fiddler](http://fiddler2.com/home), o scrivere alcuni JavaScript, per registrare un `Secret` modulo valore. Senza il [associare](https://msdn.microsoft.com/en-us/library/system.web.mvc.bindattribute(v=vs.108).aspx) attributo limitando i campi che il gestore di associazione del modello viene utilizzato quando viene creato un `Student` istanza*,* lo strumento di associazione del modello preleverebbe che `Secret` formato valore e utilizzarlo per creare il `Student` istanza di entità. Quindi indipendentemente dal valore specificato per hacker il `Secret` campo modulo verranno aggiornato nel database. La figura seguente mostra il fiddler strumento aggiungendo il `Secret` campo (con il valore "OverPost") per i valori del form inseriti.

    ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)  

    Il valore "OverPost" viene quindi aggiunto correttamente al `Secret` proprietà dell'oggetto inserito di riga, anche se non si intendeva mai che la pagina web in grado di impostare tale proprietà.

    È una protezione ottimale per utilizzare il `Include` parametro con il `Bind` attributo *whitelist* campi. È anche possibile usare il `Exclude` parametro *blacklist* campi che si desidera escludere. Il motivo `Include` è più sicuro è che, quando si aggiunge una nuova proprietà all'entità, il nuovo campo non automaticamente protetti da un `Exclude` elenco.

    È possibile impedire overposting negli scenari di modifica è lettura innanzitutto l'entità dal database e quindi chiamando `TryUpdateModel`, passando un elenco di proprietà consentiti esplicite. Si tratta del metodo utilizzato in queste esercitazioni.

    Un modo alternativo per impedire overposting che è preferibile utilizzare molti sviluppatori è usare visualizzazione di modelli anziché le classi di entità con l'associazione di modelli. Includere solo le proprietà che si desidera aggiornare il modello di visualizzazione. Al termine il raccoglitore di modelli MVC, copiare le proprietà del modello di visualizzazione per l'istanza di entità, eventualmente utilizzando uno strumento come [AutoMapper](http://automapper.org/). Utilizzare db. Voce sull'istanza di entità per impostare il proprio stato su Unchanged e quindi impostare Property("PropertyName"). IsModified su true in ogni proprietà di entità che è incluso nel modello di visualizzazione. Questo funzionamento del metodo sia in modifica e creare scenari.

    Diverso dal `Bind` attributo, il `try-catch` blocco è l'unica modifica apportate al codice di scaffolding. Se un'eccezione che deriva da [DataException](https://msdn.microsoft.com/en-us/library/system.data.dataexception.aspx) è rilevata durante il salvataggio delle modifiche, viene visualizzato un messaggio di errore generico. [DataException](https://msdn.microsoft.com/en-us/library/system.data.dataexception.aspx) eccezioni talvolta sono causate da un elemento esterno per l'applicazione, anziché un errore di programmazione, pertanto l'utente è consigliabile ripetere l'operazione. Anche se non è implementato in questo esempio, un'applicazione di qualità di produzione l'accesso utilizzando l'eccezione. Per ulteriori informazioni, vedere il **Log per informazioni dettagliate** sezione [monitoraggio e telemetria (compilazione reali le app Cloud mediante Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log).

    Il codice in *Views\Student\Create.cshtml* è simile a quello visualizzato *Details.cshtml*, ad eccezione del fatto che `EditorFor` e `ValidationMessageFor` helper vengono utilizzati per ogni campo anziché `DisplayFor`. Ecco il codice pertinente:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cshtml)]

    *Create.chstml* include anche `@Html.AntiForgeryToken()`, che funziona con il `ValidateAntiForgeryToken` attributo nel controller per evitare che [intersito richieste false](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) attacchi.

    Non sono necessarie modifiche in *Create.cshtml*.
2. Eseguire la pagina selezionando il **studenti** scheda e facendo clic su **Crea nuovo**.
3. Immettere i nomi e una data non valida e fare clic su **crea** per visualizzare il messaggio di errore.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)

    Si tratta di convalida sul lato server che si ottiene per impostazione predefinita. in un'esercitazione successiva si noterà come aggiungere gli attributi che generano anche il codice per la convalida lato client. Il codice evidenziato di seguito viene illustrato il controllo di convalida nel modello di **crea** metodo.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs?highlight=1)]
4. Modificare la data in un valore valido e fare clic su **crea** per vedere il nuovo studente nel **indice** pagina.

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

## <a name="update-the-edit-httppost-method"></a>Aggiornare il metodo HttpPost modifica

In *Controllers\StudentController.cs*, `HttpGet` `Edit` (metodo) (quello senza il `HttpPost` attributo) utilizza il `Find` metodo per recuperare l'oggetto selezionato `Student` entità, come è stato illustrato nel `Details` metodo. Non è necessario modificare questo metodo.

Tuttavia, sostituire il `HttpPost` `Edit` il metodo di azione con il codice seguente:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

Queste modifiche di sicurezza consigliata per evitare di implementare [overposting](#overpost), il scaffolder generato un `Bind` attributo e aggiungere l'entità creata dal gestore di associazione del modello per il set con un flag di modifica di entità. Codice non è più consigliato perché il `Bind` attributo Cancella tutti i dati esistenti nei campi non elencati nel `Include` parametro. In futuro, verrà aggiornato il scaffolder controller MVC in modo che non genera `Bind` attributi per i metodi di modifica.

Il nuovo codice legge di entità esistente e chiama [TryUpdateModel](https://msdn.microsoft.com/en-us/library/system.web.mvc.controller.tryupdatemodel(v=vs.118).aspx) per aggiornare i campi dall'input dell'utente nei dati del form inseriti. Rilevamento delle modifiche automatico set del Entity Framework il [Modified](https://msdn.microsoft.com/en-us/library/system.data.entitystate.aspx) flag nell'entità. Quando il [SaveChanges](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metodo viene chiamato, il `Modified` flag fa Entity Framework creare istruzioni SQL per aggiornare la riga di database. [I conflitti di concorrenza](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) vengono ignorati e vengono aggiornate tutte le colonne della riga di database, inclusi quelli che l'utente non è stato modificato. (Un'esercitazione successiva viene illustrato come gestire i conflitti di concorrenza e, se si vuole solo singoli campi da aggiornare nel database, è possibile impostare l'entità su Unchanged e singoli campi modificati.)

Come procedura consigliata per evitare di overposting, i campi che si desidera siano aggiornabili nella pagina di modifica sono inclusi nel `TryUpdateModel` parametri. Attualmente non esistono campi aggiuntivi che si vuole proteggere, ma l'elenco di campi che si desidera lo strumento di associazione per l'associazione assicura che se si aggiungono campi al modello di dati in futuro, che siano automaticamente protetti fino a quando non vengono aggiunti esplicitamente qui.

In seguito a queste modifiche, la firma del metodo del metodo HttpPost modifica corrisponde a quello del metodo di modifica HttpGet. pertanto è stato rinominato il metodo EditPost.

> [!TIP] 
> 
> **Stati di entità e il collegamento e i metodi di SaveChanges**
> 
> Il contesto di database tiene traccia del fatto che le entità in memoria vengono sincronizzate con le relative righe corrispondenti nel database e questa informazione determina cosa accade quando si chiama il `SaveChanges` metodo. Ad esempio, quando si passa una nuova entità per il [Aggiungi](https://msdn.microsoft.com/en-us/library/system.data.entity.dbset.add(v=vs.103).aspx) (metodo), che lo stato dell'entità è impostato su `Added`. Quando si chiama il [SaveChanges](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) (metodo), il contesto del database SQL di problemi `INSERT` comando.
> 
> Un'entità potrebbe essere in uno del[seguenti stati](https://msdn.microsoft.com/en-us/library/system.data.entitystate.aspx):
> 
> - `Added`. L'entità non esiste ancora nel database. Il `SaveChanges` metodo deve eseguire un `INSERT` istruzione.
> - `Unchanged`. Non deve essere eseguita con questa entità per il `SaveChanges` metodo. Quando si legga un'entità dal database, l'entità che inizia con questo stato.
> - `Modified`. Sono stati modificati alcuni o tutti i valori di proprietà dell'entità. Il `SaveChanges` metodo deve eseguire un `UPDATE` istruzione.
> - `Deleted`. L'entità è stato contrassegnato per l'eliminazione. Il `SaveChanges` metodo deve eseguire un `DELETE` istruzione.
> - `Detached`. L'entità non viene rilevato dal contesto del database.
> 
> In un'applicazione desktop, le modifiche dello stato vengono in genere impostate automaticamente. In un tipo desktop dell'applicazione, un'entità di leggere e modificare alcuni valori delle relative proprietà. In questo modo lo stato dell'entità viene convertito automaticamente in `Modified`. Quando si chiama `SaveChanges`, Entity Framework genera l'errore SQL `UPDATE` istruzione che aggiorna solo le proprietà effettive che è stato modificato.
> 
> Questa sequenza continua non consentono la natura disconnessa di App web. Il [DbContext](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=VS.103).aspx) che legge un'entità viene eliminata dopo il rendering di una pagina. Quando il `HttpPost` `Edit` viene chiamato il metodo di azione, viene effettuata una richiesta di nuovo e si dispone di una nuova istanza del [DbContext](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=VS.103).aspx), pertanto è necessario impostare manualmente lo stato dell'entità `Modified.` quindi quando si chiama `SaveChanges`, Entity Framework Aggiorna tutte le colonne della riga di database, poiché il contesto non ha modo di conoscere le proprietà che è stato modificato.
> 
> Se si desidera che l'istruzione SQL `Update` istruzione per aggiornare solo i campi che l'utente ha effettivamente modificato, è possibile salvare i valori originali in qualche modo (ad esempio nascosti) in modo che siano disponibili quando la `HttpPost` `Edit` metodo viene chiamato. È possibile creare un `Student` entità utilizzando i valori originali, chiamata di `Attach` metodo con tale versione originale dell'entità, aggiornare i valori dell'entità per i nuovi valori e quindi chiamare `SaveChanges.` per ulteriori informazioni, vedere [ Stati di entità e SaveChanges](https://msdn.microsoft.com/en-us/data/jj592676) e [dati locali](https://msdn.microsoft.com/en-us/data/jj592872) nel centro per sviluppatori MSDN dati.


Il codice HTML e Razor *Views\Student\Edit.cshtml* è simile a quello visualizzato *Create.cshtml*, e non sono necessarie modifiche.

Eseguire la pagina selezionando il **studenti** scheda e quindi fare clic su un **modifica** collegamento ipertestuale.

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

Modificare alcuni dei dati e fare clic su **salvare**. Verranno visualizzati i dati modificati nella pagina di indice.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-delete-page"></a>Aggiornare la pagina di eliminazione

In *Controllers\StudentController.cs*, il codice del modello per il `HttpGet` `Delete` metodo utilizza il `Find` metodo per recuperare l'oggetto selezionato `Student` entità, come illustrato nel `Details` e `Edit` metodi. Tuttavia, per implementare un personalizzata del messaggio di errore quando la chiamata a `SaveChanges` ha esito negativo, si aggiungeranno alcune funzionalità di questo metodo e la relativa visualizzazione corrispondente.

Come è stato illustrato per l'aggiornamento e creare operazioni, le operazioni di eliminazione richiedono due metodi di azione. Il metodo viene chiamato in risposta a una richiesta GET che consente di visualizzare una vista che consente all'utente di approvare o annullare l'operazione di eliminazione. Se l'utente approva, viene creata una richiesta POST. In questo caso, il `HttpPost` `Delete` metodo viene chiamato e quindi tale metodo esegue effettivamente l'operazione di eliminazione.

Si aggiungerà un `try-catch` blocco per il `HttpPost` `Delete` metodo per gestire eventuali errori che potrebbero verificarsi quando il database viene aggiornato. Se si verifica un errore, il `HttpPost` `Delete` chiamate al metodo di `HttpGet` `Delete` metodo, passando un parametro che indica che si è verificato un errore. Il `HttpGet Delete` metodo viene quindi visualizzata nuovamente la pagina di conferma con il messaggio di errore, che consente all'utente la possibilità di annullare o ripetere l'operazione.

1. Sostituire il `HttpGet` `Delete` il metodo di azione con il codice seguente, che gestisce la segnalazione errori: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cs?highlight=1,7-10)]

    Questo codice accetta un [parametro facoltativo](https://msdn.microsoft.com/en-us/library/dd264739.aspx) che indica se il metodo è stato chiamato dopo un errore di salvare le modifiche. Questo parametro è `false` quando il `HttpGet` `Delete` metodo viene chiamato senza un precedente errore. Quando viene chiamato `HttpPost` `Delete` metodo in risposta a un errore di aggiornamento del database, il parametro è `true` e un messaggio di errore viene passato alla visualizzazione.
- Sostituire il `HttpPost` `Delete` metodo di azione (denominato `DeleteConfirmed`) con il codice seguente, che esegue l'operazione di eliminazione effettiva e rileva eventuali errori di aggiornamento del database.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

    Questo codice viene recuperato l'entità selezionata, quindi chiama il [rimuovere](https://msdn.microsoft.com/en-us/library/system.data.entity.dbset.remove(v=vs.103).aspx) per impostare lo stato dell'entità su `Deleted`. Quando `SaveChanges` viene chiamato, un database SQL `DELETE` comando viene generato. È stato modificato il nome del metodo di azione da `DeleteConfirmed` a `Delete`. Il codice di supporto temporaneo denominato il `HttpPost` `Delete` metodo `DeleteConfirmed` per fornire il `HttpPost` metodo una firma univoca. (Common Language Runtime richiede metodi di overload per disporre di parametri di metodo diverso). Ora che le firme sono univoche, è possibile utilizzare la convenzione MVC e utilizzare lo stesso nome per il `HttpPost` e `HttpGet` eliminare i metodi.

    Se il miglioramento delle prestazioni in un'applicazione con volumi elevati è una priorità, è possibile evitare una query SQL non necessaria per recuperare la riga sostituendo le righe di codice che chiamano il `Find` e `Remove` metodi con il codice seguente:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample14.cs)]

    Questo codice crea un `Student` entità utilizzando solo il valore di chiave primario e quindi imposta lo stato dell'entità `Deleted`. Questo è tutto Entity Framework necessarie per eliminare l'entità.

    Come già indicato, il `HttpGet` `Delete` metodo non comporta l'eliminazione dei dati. Esecuzione di un'operazione di eliminazione in risposta a un'operazione GET richiesta (o a tale scopo, eseguire qualsiasi operazione di modifica, creare l'operazione o qualsiasi altra operazione che modifica i dati) consente di creare un rischio per la sicurezza. Per ulteriori informazioni, vedere [ASP.NET MVC suggerimento #46-non usare eliminare collegamenti poiché creano problemi di sicurezza](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) sul blog di Stephen Walther.
- In *Views\Student\Delete.cshtml*, aggiungere un messaggio di errore tra il `h2` intestazione e il `h3` intestazione, come illustrato nell'esempio seguente:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample15.cshtml?highlight=2)]

    Eseguire la pagina selezionando il **studenti** scheda e facendo clic su un **eliminare** collegamento ipertestuale:

    ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)
- Fare clic su **eliminare**. Verrà visualizzata la pagina di indice senza gli studenti eliminato. (Verrà visualizzato un esempio di codice in esecuzione in di gestione degli errori di [esercitazione concorrenza](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md).)

## <a name="closing-database-connections"></a>Chiusura delle connessioni di Database

Per chiudere le connessioni al database e liberare le risorse che contengano appena possibile, eliminare l'istanza del contesto al termine con esso. Vale a dire perché il codice di scaffolding fornisce un [Dispose](https://msdn.microsoft.com/en-us/library/system.idisposable.dispose(v=vs.110).aspx) metodo alla fine del `StudentController` classe *StudentController.cs*, come illustrato nell'esempio seguente:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample16.cs)]

La base `Controller` già classe implementa il `IDisposable` interfaccia, in modo che questo codice aggiunge semplicemente una sostituzione per il `Dispose(bool)` metodo per eliminare in modo esplicito l'istanza del contesto.

<a id="transactions"></a>
## <a name="handling-transactions"></a>Gestione delle transazioni

Per impostazione predefinita di Entity Framework implementa in modo implicito le transazioni. Negli scenari in cui si apportano modifiche a più righe o le tabelle e quindi chiamare `SaveChanges`, Entity Framework di verificare che tutte le modifiche apportate dal tutte esito automaticamente. Se alcune modifiche sono state completate prima e quindi si verifica un errore, tali modifiche vengono automaticamente il rollback. Per scenari in cui è necessario più controllare, ad esempio, se si desidera includere operazioni eseguite all'esterno di Entity Framework in una transazione, vedere [si utilizzano le transazioni](https://msdn.microsoft.com/en-US/data/dn456843) su MSDN.

## <a name="summary"></a>Riepilogo

Ora è un set completo di pagine in cui eseguire semplici operazioni CRUD per `Student` entità. Helper MVC usato per generare elementi dell'interfaccia utente per i campi dati. Per ulteriori informazioni su helper MVC, vedere [il Rendering di un helper HTML utilizzando modulo](https://msdn.microsoft.com/en-us/library/dd410596(v=VS.98).aspx) (la pagina è per MVC 3, ma sono ancora rilevanti per MVC 5).

Nella prossima esercitazione si saranno espandere le funzionalità della pagina di indice mediante l'aggiunta di ordinamento e paging.

Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione ed è stato possibile apportare miglioramenti. È inoltre possibile richiedere nuovi argomenti in [Mostra Me come con codice](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Sono disponibili collegamenti ad altre risorse di Entity Framework in [accesso ai dati ASP.NET - risorse](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Precedente](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
[Successivo](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
