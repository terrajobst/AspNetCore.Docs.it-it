---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: Implementazione della funzionalità CRUD di base con Entity Framework nell'applicazione ASP.NET MVC (2 di 10) | Documenti Microsoft
author: tdykstra
description: L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: f7bace3f-b85a-47ff-b5fe-49e81441cdf9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: acec5c9641b1de230956478c4396d1d541fcb0eb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875324"
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application-2-of-10"></a>Implementazione della funzionalità CRUD di base con Entity Framework nell'applicazione ASP.NET MVC (2 di 10)
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 4 utilizzando Code First di Entity Framework 5 e Visual Studio 2012. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). È possibile avviare la serie di esercitazioni dall'inizio o [scaricare un progetto di avvio per questo capitolo](building-the-ef5-mvc4-chapter-downloads.md) e iniziare da qui.
> 
> > [!NOTE] 
> > 
> > Se si esegue un problema, è possibile risolvere, [scaricare il capitolo completato](building-the-ef5-mvc4-chapter-downloads.md) e provare a riprodurre il problema. In genere, è possibile trovare la soluzione al problema di mediante un confronto tra il codice per il codice completato. Per alcuni errori comuni e su come risolverli, vedere [errori e soluzioni alternative.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Nell'esercitazione precedente è stato creato un'applicazione MVC che archivia e Visualizza dati tramite Entity Framework sia LocalDB di SQL Server. In questa esercitazione verrà esaminare e personalizzare il CRUD (create, leggere, aggiornare ed eliminare) il codice che lo scaffolding di MVC il viene creata automaticamente nel controller e visualizzazioni.

> [!NOTE]
> È pratica comune implementare il modello di repository per creare un livello di astrazione tra il controller e il livello di accesso ai dati. Per semplificare queste esercitazioni, non implementare un repository finché un'esercitazione successiva di questa serie.


In questa esercitazione si creerà le pagine web seguenti:

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="creating-a-details-page"></a>Creazione di una pagina di dettagli

Il codice di supporto temporaneo per studenti `Index` pagina tralasciato il `Enrollments` proprietà, in quanto tale proprietà contiene una raccolta. Nel `Details` pagina verranno visualizzati il contenuto della raccolta in una tabella HTML.

 In *Controllers\StudentController.cs*, il metodo di azione per il `Details` visualizzare utilizza il `Find` metodo per recuperare un singolo `Student` entità. 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

 Il valore della chiave viene passato al metodo come la `id` parametro e dai dati di route il **dettagli** collegamento ipertestuale nella pagina di indice. 

1. Aprire *Views\Student\Details.cshtml*. Ogni campo viene visualizzato utilizzando un `DisplayFor` helper, come illustrato nell'esempio seguente: 

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cshtml)]
2. Dopo il `EnrollmentDate` e immediatamente prima della chiusura `fieldset` tag, aggiungere il codice per visualizzare un elenco delle registrazioni, come illustrato nell'esempio seguente:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml?highlight=4-22)]

    Il codice esegue il ciclo nelle entità nella proprietà di navigazione `Enrollments`. Per ogni `Enrollment` entità nella proprietà, viene visualizzato il titolo del corso e il livello. Il titolo del corso viene recuperato dal `Course` entità che viene archiviato nel `Course` proprietà di navigazione del `Enrollments` entità. Tutti i dati vengono recuperati dal database automaticamente quando necessario. (In altre parole, si utilizza il caricamento lazy qui. Non è stato specificato *caricamento eager* per il `Courses` proprietà di navigazione, pertanto la prima volta che si tenta di accedere a questa proprietà, viene inviata una query al database per recuperare i dati. Altre informazioni sul caricamento lazy e caricamento eager nel [lettura di dati correlati](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) esercitazione più avanti in questa serie.)
3. Eseguire la pagina selezionando il **studenti** scheda e facendo clic su un **dettagli** collegamento per Alexander Carson. Viene visualizzato l'elenco dei corsi e dei voti dello studente selezionato:

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

## <a name="updating-the-create-page"></a>Aggiornamento della pagina di creazione

1. In *Controllers\StudentController.cs*, sostituire il `HttpPost``Create` metodo di azione con il codice seguente per aggiungere un `try-catch` blocco e [attributo Bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) al metodo di supporto temporaneo: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cs?highlight=4,7-8,14-20)]

    Questo codice aggiunge il `Student` entità creata dal gestore di associazione di modelli ASP.NET MVC per il `Students` set di entità e quindi Salva le modifiche apportate al database. (*Dello strumento di associazione del modello* fa riferimento alle funzionalità ASP.NET MVC rende più semplice da utilizzare con i dati inviati da un modulo; un raccoglitore di modelli di modulo registrati converte i valori ai tipi CLR e li passa al metodo di azione nei parametri. In questo caso, lo strumento di associazione crea un `Student` entità per la proprietà utilizzando i valori di `Form` insieme.)

    Il `ValidateAntiForgeryToken` attributo consente di evitare [intersito richieste false](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) attacchi.

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
2. Eseguire la pagina selezionando il **studenti** scheda e facendo clic su **Crea nuovo**.

    ![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

    La convalida dei dati funziona per impostazione predefinita. Immettere i nomi e una data non valida e fare clic su **crea** per visualizzare il messaggio di errore.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

    Il codice evidenziato di seguito viene illustrato il controllo di convalida del modello.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=5)]

    Modificare la data in un valore valido, ad esempio 9/1/2005 e fare clic su **crea** per vedere il nuovo studente nel **indice** pagina.

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-edit-post-page"></a>Aggiornamento della pagina di modifica POST

In *Controllers\StudentController.cs*, `HttpGet` `Edit` (metodo) (quello senza il `HttpPost` attributo) utilizza il `Find` metodo per recuperare l'oggetto selezionato `Student` entità, come è stato illustrato nel `Details` metodo. Non è necessario modificare questo metodo.

Tuttavia, sostituire il `HttpPost` `Edit` metodo di azione con il codice seguente per aggiungere un `try-catch` blocco e [attributo Bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx):

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs)]

Questo codice è simile a quello visualizzato nel `HttpPost` `Create` metodo. Tuttavia, anziché aggiungere l'entità creata dal gestore di associazione del modello per il set di entità, questo codice imposta un flag per l'entità che indica che è stato modificato. Quando il [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metodo viene chiamato, il [Modified](https://msdn.microsoft.com/library/system.data.entitystate.aspx) flag fa Entity Framework creare istruzioni SQL per aggiornare la riga di database. Verranno aggiornate tutte le colonne della riga di database, inclusi quelli che l'utente non sono stati modificati, e vengono ignorati i conflitti di concorrenza. (Si apprenderà come gestire la concorrenza in un'esercitazione successiva di questa serie.)

### <a name="entity-states-and-the-attach-and-savechanges-methods"></a>Stati di entità e il collegamento e i metodi di SaveChanges

Il contesto del database rileva se le entità in memoria sono sincronizzate con le righe corrispondenti nel database e queste informazioni determinano le operazioni eseguite quando viene chiamato il metodo `SaveChanges`. Ad esempio, quando si passa una nuova entità per il [Aggiungi](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) (metodo), che lo stato dell'entità è impostato su `Added`. Quando si chiama il [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) (metodo), il contesto del database SQL di problemi `INSERT` comando.

Un'entità potrebbe essere in uno del[seguenti stati](https://msdn.microsoft.com/library/system.data.entitystate.aspx):

- `Added`. L'entità non esiste ancora nel database. Il `SaveChanges` metodo deve eseguire un `INSERT` istruzione.
- `Unchanged`. Il metodo `SaveChanges` non deve eseguire alcuna operazione con l'entità. Quando un'entità viene letta dal database, l'entità ha inizialmente questo stato.
- `Modified`. Sono stati modificati alcuni o tutti i valori di proprietà dell'entità. Il `SaveChanges` metodo deve eseguire un `UPDATE` istruzione.
- `Deleted`. L'entità è stata contrassegnata per l'eliminazione. Il `SaveChanges` metodo deve eseguire un `DELETE` istruzione.
- `Detached`. L'entità non viene registrata dal contesto del database.

In un'applicazione desktop le modifiche dello stato vengono in genere impostate automaticamente. In un tipo desktop dell'applicazione, un'entità di leggere e modificare alcuni valori delle relative proprietà. In questo modo lo stato dell'entità viene modificato automaticamente in `Modified`. Quando si chiama `SaveChanges`, Entity Framework genera l'errore SQL `UPDATE` istruzione che aggiorna solo le proprietà effettive che è stato modificato.

Questa sequenza continua non consentono la natura disconnessa di App web. Il [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) che legge un'entità viene eliminata dopo il rendering di una pagina. Quando il `HttpPost` `Edit` viene chiamato il metodo di azione, viene effettuata una richiesta di nuovo e si dispone di una nuova istanza del [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), pertanto è necessario impostare manualmente lo stato dell'entità `Modified.` quindi quando si chiama `SaveChanges`, Entity Framework Aggiorna tutte le colonne della riga di database, poiché il contesto non ha modo di conoscere le proprietà che è stato modificato.

Se si desidera che l'istruzione SQL `Update` istruzione per aggiornare solo i campi che l'utente ha effettivamente modificato, è possibile salvare i valori originali in qualche modo (ad esempio nascosti) in modo che siano disponibili quando la `HttpPost` `Edit` metodo viene chiamato. È possibile creare un `Student` entità utilizzando i valori originali, chiamata di `Attach` metodo con tale versione originale dell'entità, aggiornare i valori dell'entità per i nuovi valori e quindi chiamare `SaveChanges.` per ulteriori informazioni, vedere [ Stati di entità e SaveChanges](https://msdn.microsoft.com/data/jj592676) e [dati locali](https://msdn.microsoft.com/data/jj592872) nel centro per sviluppatori MSDN dati.

Il codice in *Views\Student\Edit.cshtml* è simile a quello visualizzato *Create.cshtml*, e non sono necessarie modifiche.

Eseguire la pagina selezionando il **studenti** scheda e quindi fare clic su un **modifica** collegamento ipertestuale.

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)

Modificare alcuni dati e fare clic su **Salva**. Verranno visualizzati i dati modificati nella pagina di indice.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Aggiornare la pagina di eliminazione

In *Controllers\StudentController.cs*, il codice del modello per il `HttpGet` `Delete` metodo utilizza il `Find` metodo per recuperare l'oggetto selezionato `Student` entità, come illustrato nel `Details` e `Edit` metodi. Tuttavia, per implementare un messaggio di errore personalizzato quando la chiamata di `SaveChanges` ha esito negativo, verrà aggiunta una funzionalità al metodo e alla visualizzazione corrispondente.

Analogamente alle operazioni di aggiornamento e creazione, le operazioni di eliminazione richiedono due metodi di azione. Il metodo viene chiamato in risposta a una richiesta GET che consente di visualizzare una vista che consente all'utente di approvare o annullare l'operazione di eliminazione. Se l'utente approva, viene creata una richiesta POST. In questo caso, il `HttpPost` `Delete` metodo viene chiamato e quindi tale metodo esegue effettivamente l'operazione di eliminazione.

Si aggiungerà un `try-catch` blocco per il `HttpPost` `Delete` metodo per gestire eventuali errori che potrebbero verificarsi quando il database viene aggiornato. Se si verifica un errore, il `HttpPost` `Delete` chiamate al metodo di `HttpGet` `Delete` metodo, passando un parametro che indica che si è verificato un errore. Il `HttpGet Delete` metodo viene quindi visualizzata nuovamente la pagina di conferma con il messaggio di errore, che consente all'utente la possibilità di annullare o ripetere l'operazione.

1. Sostituire il `HttpGet` `Delete` il metodo di azione con il codice seguente, che gestisce la segnalazione errori: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cs)]

    Questo codice accetta un [facoltativo](https://msdn.microsoft.com/library/dd264739.aspx) parametro booleano che indica se è stata chiamata dopo un errore di salvare le modifiche. Questo parametro è `false` quando il `HttpGet` `Delete` metodo viene chiamato senza un precedente errore. Quando viene chiamato `HttpPost` `Delete` metodo in risposta a un errore di aggiornamento del database, il parametro è `true` e un messaggio di errore viene passato alla visualizzazione.
2. Sostituire il `HttpPost` `Delete` metodo di azione (denominato `DeleteConfirmed`) con il codice seguente, che esegue l'operazione di eliminazione effettiva e rileva eventuali errori di aggiornamento del database.

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs)]

     Questo codice viene recuperato l'entità selezionata, quindi chiama il [rimuovere](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) per impostare lo stato dell'entità su `Deleted`. Quando `SaveChanges` viene chiamato, un database SQL `DELETE` comando viene generato. Anche il nome del metodo di azione è stato modificato da `DeleteConfirmed` a `Delete`. Il codice di supporto temporaneo denominato il `HttpPost` `Delete` metodo `DeleteConfirmed` per fornire il `HttpPost` metodo una firma univoca. (Common Language Runtime richiede metodi di overload per disporre di parametri di metodo diverso). Ora che le firme sono univoche, è possibile utilizzare la convenzione MVC e utilizzare lo stesso nome per il `HttpPost` e `HttpGet` eliminare i metodi.

     Se il miglioramento delle prestazioni in un'applicazione con volumi elevati è una priorità, è possibile evitare una query SQL non necessaria per recuperare la riga sostituendo le righe di codice che chiamano il `Find` e `Remove` metodi con il codice seguente come illustrato in giallo evidenziare:

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

     Questo codice crea un `Student` entità utilizzando solo il valore di chiave primario e quindi imposta lo stato dell'entità `Deleted`. Questo è tutto ciò di cui Entity Framework necessita per eliminare l'entità.

     Come già indicato, il `HttpGet` `Delete` metodo non comporta l'eliminazione dei dati. Esecuzione di un'operazione di eliminazione in risposta a un'operazione GET richiesta (o a tale scopo, eseguire qualsiasi operazione di modifica, creare l'operazione o qualsiasi altra operazione che modifica i dati) consente di creare un rischio per la sicurezza. Per ulteriori informazioni, vedere [ASP.NET MVC suggerimento #46-non usare eliminare collegamenti poiché creano problemi di sicurezza](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) sul blog di Stephen Walther.
3. In *Views\Student\Delete.cshtml*, aggiungere un messaggio di errore tra il `h2` intestazione e il `h3` intestazione, come illustrato nell'esempio seguente:

     [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cshtml?highlight=2)]

     Eseguire la pagina selezionando il **studenti** scheda e facendo clic su un **eliminare** collegamento ipertestuale:

     ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image12.png)
4. Fare clic su **Elimina**. Viene visualizzata la pagina Index senza lo studente eliminato. (Verrà visualizzato un esempio di codice in esecuzione in di gestione degli errori di [la gestione della concorrenza](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) esercitazione più avanti in questa serie.)

## <a name="ensuring-that-database-connections-are-not-left-open"></a>Verificare che le connessioni al Database non vengono lasciate aperte

Per assicurarsi che le connessioni al database vengano chiuse correttamente e le risorse contengano liberati backup, verrà visualizzato a tale che l'istanza del contesto è stato eliminato. Vale a dire perché il codice di scaffolding fornisce un [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) metodo alla fine del `StudentController` classe *StudentController.cs*, come illustrato nell'esempio seguente:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

La base `Controller` già classe implementa il `IDisposable` interfaccia, in modo che questo codice aggiunge semplicemente una sostituzione per il `Dispose(bool)` metodo per eliminare in modo esplicito l'istanza del contesto.

## <a name="summary"></a>Riepilogo

Ora è un set completo di pagine in cui eseguire semplici operazioni CRUD per `Student` entità. Helper MVC usato per generare elementi dell'interfaccia utente per i campi dati. Per ulteriori informazioni su helper MVC, vedere [il Rendering di un helper HTML utilizzando modulo](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx) (la pagina è per MVC 3, ma sono ancora rilevanti per MVC 4).

Nella prossima esercitazione si saranno espandere le funzionalità della pagina di indice mediante l'aggiunta di ordinamento e paging.

Collegamenti ad altre risorse di Entity Framework, vedere il [mappa del contenuto ASP.NET dati accesso](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Precedente](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
> [Successivo](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
