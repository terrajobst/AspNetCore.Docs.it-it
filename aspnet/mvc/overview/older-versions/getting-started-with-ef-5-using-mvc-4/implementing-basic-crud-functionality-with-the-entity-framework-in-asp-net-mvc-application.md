---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: Implementazione della funzionalità CRUD di base con Entity Framework nell'applicazione ASP.NET MVC (2 di 10) | Microsoft Docs
author: tdykstra
description: L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio...
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: f7bace3f-b85a-47ff-b5fe-49e81441cdf9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: f580ba6d0db07430e991fba2d061bb2632c95353
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817083"
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application-2-of-10"></a>Implementazione della funzionalità CRUD di base con Entity Framework nell'applicazione ASP.NET MVC (2 di 10)
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto completato](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio 2012. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). È possibile avviare la serie di esercitazioni a partire dall'inizio oppure [scarica un progetto iniziale per questo capitolo](building-the-ef5-mvc4-chapter-downloads.md) e iniziare da qui.
> 
> > [!NOTE] 
> > 
> > Se si verifica un problema è possibile risolvere, [Scarica il capitolo completato](building-the-ef5-mvc4-chapter-downloads.md) e provare a riprodurre il problema. Confrontando il codice per il codice completo è generalmente possibile trovare la soluzione al problema. Per alcuni errori comuni e come risolverli, vedere [errori e soluzioni alternative.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Nell'esercitazione precedente è creata un'applicazione MVC che memorizza e Visualizza dati tramite Entity Framework e SQL Server LocalDB. In questa esercitazione verrà esaminato e personalizzare il CRUD (create, leggere, aggiornare ed eliminare) il codice che lo scaffolding di MVC crea automaticamente per l'utente nel controller e visualizzazioni.

> [!NOTE]
> È pratica comune implementare il modello di repository per creare un livello di astrazione tra il controller e il livello di accesso ai dati. Per semplificare queste esercitazioni, è non implementare un repository fino a un'esercitazione successiva di questa serie.


In questa esercitazione si creerà le pagine web seguenti:

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="creating-a-details-page"></a>Creazione di una pagina di dettagli

Il codice con scaffolding per studenti `Index` pagina, esclusi il `Enrollments` proprietà, poiché la proprietà contiene una raccolta. Nel `Details` pagina verranno visualizzati il contenuto della raccolta in una tabella HTML.

 Nelle *Controllers\StudentController.cs*, il metodo di azione per il `Details` visualizzare Usa il `Find` metodo per recuperare una singola `Student` entità. 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

 Il valore della chiave viene passato al metodo come la `id` parametro e viene fornita dai dati di route il **dettagli** collegamento ipertestuale nella pagina di indice. 

1. Aprire *Views\Student\Details.cshtml*. Ogni campo viene visualizzato utilizzando un `DisplayFor` helper, come illustrato nell'esempio seguente: 

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cshtml)]
2. Dopo il `EnrollmentDate` campo e immediatamente prima della chiusura `fieldset` tag, aggiungere il codice per visualizzare un elenco delle registrazioni, come illustrato nell'esempio seguente:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml?highlight=4-22)]

    Il codice esegue il ciclo nelle entità nella proprietà di navigazione `Enrollments`. Per ogni `Enrollment` entità nella proprietà, viene visualizzato il titolo del corso e il livello. Il titolo del corso viene recuperato dal `Course` entità a cui viene archiviato nel `Course` proprietà di navigazione del `Enrollments` entità. Tutti i dati vengono recuperati dal database automaticamente quando è necessaria. (In altre parole, si usa il caricamento lazy qui. Non è stato specificato *caricamento eager* per il `Courses` proprietà di navigazione, in modo che la prima volta che si tenta di accedere a questa proprietà, viene inviata una query al database per recuperare i dati. Altre informazioni sulle modalità di caricamento lazy e il caricamento eager nel [lettura di dati correlati](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) esercitazione più avanti in questa serie.)
3. Eseguire la pagina selezionando le **studenti** scheda e facendo clic su un **dettagli** collegamento per Alexander Carson. Viene visualizzato l'elenco dei corsi e dei voti dello studente selezionato:

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

## <a name="updating-the-create-page"></a>Aggiornare la pagina Create

1. Nelle *Controllers\StudentController.cs*, sostituire il `HttpPost``Create` metodo di azione con il codice seguente per aggiungere un `try-catch` blocco e il [attributo binding](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) al metodo sottoposto a scaffolding: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cs?highlight=4,7-8,14-20)]

    Questo codice aggiunge il `Student` entità creata dallo strumento di associazione di modelli ASP.NET MVC per la `Students` entità impostata e quindi Salva le modifiche apportate al database. (*Dello strumento di associazione del modello* fa riferimento a funzionalità di ASP.NET MVC che rende più semplice per lavorare con i dati inviati da un modulo; uno strumento di associazione di modello modulo converte registrati i valori per i tipi CLR e li passa al metodo di azione nei parametri. In questo caso, lo strumento di associazione di modello crea un'istanza una `Student` entità con proprietà i valori di `Form` raccolta.)

    Il `ValidateAntiForgeryToken` attributo consente di impedire [XSS richiesta intersito falsa](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) attacchi.

<a id="overpost"></a>

    > [!WARNING]
    > Security - The `Bind` attribute is added to protect against *over-posting*. For example, suppose the `Student` entity includes a `Secret` property that you don't want this web page to update.
    > 
    > [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cs?highlight=7)]
    > 
    > Even if you don't have a `Secret` field on the web page, a hacker could use a tool such as [fiddler](http://fiddler2.com/home), or write some JavaScript, to post a `Secret` form value. Without the [Bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) attribute limiting the fields that the model binder uses when it creates a `Student` instance*,* the model binder would pick up that `Secret` form value and use it to update the `Student` entity instance. Then whatever value the hacker specified for the `Secret` form field would be updated in your database. The following image shows the fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.
    > 
    > ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)  
    > 
    > The value "OverPost" would then be successfully added to the `Secret` property of the inserted row, although you never intended that the web page be able to update that property.
    > 
    > It's a security best practice to use the `Include` parameter with the `Bind` attribute to *whitelist* fields. It's also possible to use the `Exclude` parameter to *blacklist* fields you want to exclude. The reason `Include` is more secure is that when you add a new property to the entity, the new field is not automatically protected by an `Exclude` list.
    > 
    > Another alternative approach, and one preferred by many, is to use only view models with model binding. The view model contains only the properties you want to bind. Once the MVC model binder has finished, you copy the view model properties to the entity instance.

    Other than the `Bind` attribute, the `try-catch` block is the only change you've made to the scaffolded code. If an exception that derives from [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) is caught while the changes are being saved, a generic error message is displayed. [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) exceptions are sometimes caused by something external to the application rather than a programming error, so the user is advised to try again. Although not implemented in this sample, a production quality application would log the exception (and non-null inner exceptions ) with a logging mechanism such as [ELMAH](https://code.google.com/p/elmah/).

    The code in *Views\Student\Create.cshtml* is similar to what you saw in *Details.cshtml*, except that `EditorFor` and `ValidationMessageFor` helpers are used for each field instead of `DisplayFor`. The following example shows the relevant code:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml)]

    *Create.chstml* also includes `@Html.AntiForgeryToken()`, which works with the `ValidateAntiForgeryToken` attribute in the controller to help prevent [cross-site request forgery](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) attacks.

    No changes are required in *Create.cshtml*.
2. Eseguire la pagina selezionando le **studenti** scheda e facendo clic su **Crea nuovo**.

    ![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

    La convalida dei dati funziona per impostazione predefinita. Immettere i nomi e una data non valida e fare clic su **Create** per visualizzare il messaggio di errore.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

    Il codice evidenziato seguente illustra il controllo di convalida del modello.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=5)]

    Modificare la data di un valore valido, ad esempio 9/1/2005 e fare clic su **Create** per visualizzare il nuovo studente nella **indice** pagina.

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-edit-post-page"></a>Aggiorna la pagina di modifica POST

In *Controllers\StudentController.cs*, il `HttpGet` `Edit` metodo (quello senza il `HttpPost` attributo) usa il `Find` metodo per recuperare l'oggetto selezionato `Student` entità, come è stato illustrato nel `Details` (metodo). Non è necessario modificare questo metodo.

Tuttavia, sostituire il `HttpPost` `Edit` metodo di azione con il codice seguente per aggiungere un `try-catch` blocco e il [attributo binding](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx):

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs)]

Questo codice è simile a quello illustrato nel `HttpPost` `Create` (metodo). Tuttavia, anziché aggiungere l'entità creata dallo strumento di associazione modelli al set di entità, questo codice imposta un flag per l'entità che indica che è stato modificato. Quando la [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) viene chiamato, il [Modified](https://msdn.microsoft.com/library/system.data.entitystate.aspx) flag fa in modo che Entity Framework creare istruzioni SQL per aggiornare la riga del database. Tutte le colonne della riga di database verranno aggiornate, inclusi quelli che l'utente non è stato modificato, e i conflitti di concorrenza vengono ignorati. (Verrà illustrato come gestire la concorrenza in un'esercitazione successiva di questa serie).

### <a name="entity-states-and-the-attach-and-savechanges-methods"></a>Stati di entità e le collega e i metodi SaveChanges

Il contesto del database rileva se le entità in memoria sono sincronizzate con le righe corrispondenti nel database e queste informazioni determinano le operazioni eseguite quando viene chiamato il metodo `SaveChanges`. Ad esempio, quando si passa una nuova entità per il [Add](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) metodo, lo stato dell'entità viene impostato su `Added`. Quando si chiama il [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metodo, il contesto del database esegue un database SQL `INSERT` comando.

Le entità possono essere in uno dei[seguendo gli stati](https://msdn.microsoft.com/library/system.data.entitystate.aspx):

- `Added`. L'entità non esiste ancora nel database. Il `SaveChanges` metodo deve generare un `INSERT` istruzione.
- `Unchanged`. Il metodo `SaveChanges` non deve eseguire alcuna operazione con l'entità. Quando un'entità viene letta dal database, l'entità ha inizialmente questo stato.
- `Modified`. Sono stati modificati alcuni o tutti i valori di proprietà dell'entità. Il `SaveChanges` metodo deve generare un `UPDATE` istruzione.
- `Deleted`. L'entità è stata contrassegnata per l'eliminazione. Il `SaveChanges` metodo deve generare un `DELETE` istruzione.
- `Detached`. L'entità non viene registrata dal contesto del database.

In un'applicazione desktop le modifiche dello stato vengono in genere impostate automaticamente. In un tipo desktop dell'applicazione, viene letta un'entità e apportare modifiche ad alcuni dei valori delle proprietà. In questo modo lo stato dell'entità viene modificato automaticamente in `Modified`. Quando si chiama `SaveChanges`, Entity Framework genera un codice SQL `UPDATE` istruzione che aggiorna solo le proprietà che è stato modificato.

Per questa sequenza continua non consente la natura disconnessa delle App web. Il [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) che legge un'entità viene eliminata dopo il rendering di una pagina. Quando la `HttpPost` `Edit` viene chiamato il metodo di azione, viene effettuata una nuova richiesta e si dispone di una nuova istanza della [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), pertanto è necessario impostare manualmente lo stato dell'entità `Modified.` quindi quando si chiama `SaveChanges`, Entity Framework Aggiorna tutte le colonne della riga di database, poiché il contesto non ha modo di conoscere le proprietà modificate.

Se si desidera che il codice SQL `Update` istruzione per aggiornare solo i campi modificati dall'utente, è possibile salvare i valori originali in qualche modo (ad esempio campi nascosti) in modo che siano disponibili quando il `HttpPost` `Edit` viene chiamato il metodo. È possibile creare un `Student` entità usando i valori originali, chiamare il `Attach` metodo con tale versione originale dell'entità, aggiornare i valori dell'entità per i nuovi valori e quindi chiamare `SaveChanges.` per altre informazioni, vedere [ Stati di entità e SaveChanges](https://msdn.microsoft.com/data/jj592676) e [i dati locali](https://msdn.microsoft.com/data/jj592872) in MSDN Data Developer Center.

Il codice nel *Views\Student\Edit.cshtml* è simile a quello illustrato *create. cshtml*, e non sono necessarie modifiche.

Eseguire la pagina selezionando le **studenti** scheda e quindi facendo clic su un **modificare** collegamento ipertestuale.

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)

Modificare alcuni dati e fare clic su **Salva**. Noterete che i dati modificati nella pagina di indice.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Aggiornare la pagina Delete

In *Controllers\StudentController.cs*, il codice del modello per il `HttpGet` `Delete` metodo Usa il `Find` metodo per recuperare l'oggetto selezionato `Student` entità, come illustrato nella `Details` e `Edit` metodi. Tuttavia, per implementare un messaggio di errore personalizzato quando la chiamata di `SaveChanges` ha esito negativo, verrà aggiunta una funzionalità al metodo e alla visualizzazione corrispondente.

Analogamente alle operazioni di aggiornamento e creazione, le operazioni di eliminazione richiedono due metodi di azione. Il metodo che viene chiamato in risposta a una richiesta GET apre una visualizzazione che consente all'utente di approvare o annullare l'operazione di eliminazione. Se l'utente approva, viene creata una richiesta POST. In questo caso, il `HttpPost` `Delete` viene chiamato il metodo e quindi tale metodo esegue effettivamente l'operazione di eliminazione.

Si aggiungerà un `try-catch` bloccare per il `HttpPost` `Delete` metodo per gestire eventuali errori che potrebbero verificarsi quando il database viene aggiornato. Se si verifica un errore, il `HttpPost` `Delete` chiamate al metodo il `HttpGet` `Delete` metodo, passando un parametro che indica che si è verificato un errore. Il `HttpGet Delete` metodo visualizza nuovamente la pagina di conferma con il messaggio di errore offrendo all'utente la possibilità di annullare o ripetere l'operazione.

1. Sostituire il `HttpGet` `Delete` metodo di azione con il codice seguente, che gestisce la segnalazione errori: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cs)]

    Questo codice accetta un [facoltativi](https://msdn.microsoft.com/library/dd264739.aspx) parametro booleano che indica se questo è stato chiamato dopo un errore durante il salvataggio delle modifiche. Questo parametro è `false` quando il `HttpGet` `Delete` viene chiamato senza un precedente errore. Quando viene chiamato il `HttpPost` `Delete` metodo in risposta a un errore di aggiornamento del database, il parametro è `true` e un messaggio di errore viene passato alla visualizzazione.
2. Sostituire il `HttpPost` `Delete` metodo di azione (denominato `DeleteConfirmed`) con il codice seguente, che esegue l'operazione di eliminazione e rileva eventuali errori di aggiornamento del database.

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs)]

     Questo codice recupera l'entità selezionata, quindi chiama il [rimuovere](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) metodo per impostare lo stato dell'entità su `Deleted`. Quando `SaveChanges` viene chiamato, un database SQL `DELETE` comando viene generato. Anche il nome del metodo di azione è stato modificato da `DeleteConfirmed` a `Delete`. Il nome in codice con scaffolding il `HttpPost` `Delete` metodo `DeleteConfirmed` per fornire la `HttpPost` metodo una firma univoca. (Il CLR richiede metodi di overload per avere parametri di metodo diversi). Ora che le firme sono univoche, è possibile utilizzare la convenzione di MVC e usare lo stesso nome per il `HttpPost` e `HttpGet` metodi di eliminazione.

     Se il miglioramento delle prestazioni in un'applicazione di grandi volumi è una priorità, è possibile evitare una query SQL non necessaria per recuperare la riga da sostituire le righe di codice che chiamano il `Find` e `Remove` metodi con il codice seguente come illustrato in giallo evidenziare:

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

     Questo codice crea un `Student` entità usando solo il valore di chiave primaria e quindi imposta lo stato dell'entità `Deleted`. Questo è tutto ciò di cui Entity Framework necessita per eliminare l'entità.

     Come accennato, il `HttpGet` `Delete` metodo non comporta l'eliminazione dei dati. Esecuzione di un'operazione di eliminazione in risposta a una richiesta GET request (o per l'esecuzione di qualsiasi operazione di modifica, creazione o qualsiasi altra operazione che modifica i dati) crea un rischio per la sicurezza. Per altre informazioni, vedere [46 & suggerimento di ASP.NET MVC, non usare i collegamenti di eliminazione poiché creano le falle](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) sul blog di Stephen Walther.
3. Nelle *Views\Student\Delete.cshtml*, aggiungere un messaggio di errore tra il `h2` intestazione e il `h3` intestazione, come illustrato nell'esempio seguente:

     [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cshtml?highlight=2)]

     Eseguire la pagina selezionando le **studenti** scheda e facendo clic su un **eliminare** collegamento ipertestuale:

     ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image12.png)
4. Fare clic su **Elimina**. Viene visualizzata la pagina Index senza lo studente eliminato. (È disponibile un esempio del codice in azione nella gestione degli errori di [la gestione della concorrenza](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) esercitazione più avanti in questa serie.)

## <a name="ensuring-that-database-connections-are-not-left-open"></a>Garantire che le connessioni al Database non vengono lasciate aperte

Per assicurarsi che le connessioni di database vengano chiuse correttamente e le risorse che contengono liberati backup, si dovrebbe vedere a tale che l'istanza del contesto viene eliminato. Vale a dire il motivo per cui il codice con scaffolding include un [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) alla fine del metodo le `StudentController` classe *StudentController.cs*, come illustrato nell'esempio seguente:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

La base `Controller` classe già implementa le `IDisposable` interfaccia, in modo che questo codice aggiunge semplicemente una sostituzione per il `Dispose(bool)` metodo per eliminare in modo esplicito l'istanza del contesto.

## <a name="summary"></a>Riepilogo

Ora è un set completo di pagine che eseguono operazioni CRUD semplici per `Student` entità. Helper di MVC è utilizzato per generare gli elementi dell'interfaccia utente per i campi dati. Per altre informazioni sull'helper di MVC, vedere [il Rendering di un helper HTML usando Form](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx) (la pagina è MVC 3, ma sono ancora rilevanti per MVC 4).

Nella prossima esercitazione si verrà estesa la funzionalità della pagina dell'indice mediante l'aggiunta di ordinamento e paging.

Sono disponibili collegamenti ad altre risorse di Entity Framework nel [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Precedente](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
> [Successivo](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
