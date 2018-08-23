---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: Gestione della concorrenza con Entity Framework in un'applicazione ASP.NET MVC (7 di 10) | Microsoft Docs
author: tdykstra
description: L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: b83f47c4-8521-4d0a-8644-e8f77e39733e
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 07d4d673b7bb6bad6e9d8cbacbc965a60608db2a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827412"
---
<a name="handling-concurrency-with-the-entity-framework-in-an-aspnet-mvc-application-7-of-10"></a>Gestione della concorrenza con Entity Framework in un'applicazione ASP.NET MVC (7 di 10)
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto completato](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio 2012. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). È possibile avviare la serie di esercitazioni a partire dall'inizio oppure [scarica un progetto iniziale per questo capitolo](building-the-ef5-mvc4-chapter-downloads.md) e iniziare da qui.
> 
> > [!NOTE] 
> > 
> > Se si verifica un problema è possibile risolvere, [Scarica il capitolo completato](building-the-ef5-mvc4-chapter-downloads.md) e provare a riprodurre il problema. Confrontando il codice per il codice completo è generalmente possibile trovare la soluzione al problema. Per alcuni errori comuni e come risolverli, vedere [errori e soluzioni alternative.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Nelle due esercitazioni precedenti si è lavorato con i dati correlati. Questa esercitazione illustra come gestire la concorrenza. Creerete pagine web che funzionano con il `Department` entità e le pagine edit e delete `Department` entità gestirà gli errori di concorrenza. Le illustrazioni seguenti mostrano le pagine di indice e Delete, inclusi alcuni messaggi che vengono visualizzati se si verifica un conflitto di concorrenza.

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>Conflitti di concorrenza

Un conflitto di concorrenza si verifica quando un utente visualizza i dati di un'entità per modificarli mentre un altro utente aggiorna i dati della stessa entità prima che la modifica del primo utente venga scritta nel database. Se non si abilita il rilevamento di questi conflitti, l'ultimo utente che aggiorna il database sovrascrive le modifiche apportate dall'altro utente. In molte applicazioni questo rischio è accettabile: se il numero di utenti è ridotto o se l'eventuale sovrascrittura di alcune modifiche non è un aspetto critico, i costi della programmazione per la concorrenza possono superare i vantaggi. In tal caso non è necessario configurare l'applicazione per la gestione dei conflitti di concorrenza.

### <a name="pessimistic-concurrency-locking"></a>Concorrenza pessimistica (blocco)

Se è importante che l'applicazione eviti la perdita accidentale di dati in scenari di concorrenza, un metodo per garantire che ciò accada è l'uso dei blocchi di database. Questa operazione viene definita *concorrenza pessimistica*. Ad esempio, prima di leggere una riga da un database si richiede un blocco per l'accesso di sola lettura o per l'accesso in modalità aggiornamento. Se si blocca una riga per l'accesso di aggiornamento, nessun altro utente può bloccare la riga per l'accesso di sola lettura o di aggiornamento, perché riceverebbe una copia di dati dei quali è in corso la modifica. Se si blocca una riga per l'accesso in sola lettura, anche altri utenti possono bloccarla per l'accesso in sola lettura, ma non per l'aggiornamento.

La gestione dei blocchi presenta svantaggi. La sua programmazione può risultare complicata. Richiede risorse di gestione di database significativa, e può causare problemi di prestazioni come il numero di utenti di un'applicazione aumenta (vale a dire non è facilmente scalabile). Per questi motivi non tutti i sistemi di gestione di database supportano la concorrenza pessimistica. Entity Framework non fornisce alcun supporto predefinito per questa e in questa esercitazione non illustra come implementarla.

### <a name="optimistic-concurrency"></a>Concorrenza ottimistica

È l'alternativa alla concorrenza pessimistica *la concorrenza ottimistica*. Nella concorrenza ottimistica si consente che i conflitti di concorrenza si verifichino, quindi si reagisce con le modalità appropriate. Ad esempio, Giorgio esegue la pagina di modifica di reparti, le modifiche il **Budget** quantità per il reparto English da $350.000,00 a $0,00.

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Prima di John fa clic su **salvare**, Jane esegue la stessa pagina e le modifiche le **Start Date** 9/1/2007 campo a 8 o 8/2013.

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

John fa clic su **salvare** prima e vede sua modifica quando il browser torna alla pagina di indice, Jane quindi fa clic su **salvare**. Le operazioni successive dipendono da come si decide di gestire i conflitti di concorrenza. Di seguito sono elencate alcune opzioni:

- È possibile tenere traccia della proprietà che un utente ha modificato e aggiornare solo le colonne corrispondenti nel database. Nello scenario dell'esempio non si perde nessun dato, perché i due utenti hanno aggiornato proprietà diverse. La volta successiva che un utente torna a visualizzare il reparto English, verranno visualizzate le modifiche sia di John e Jane, ovvero una data di inizio di 8 o 8/2013 e un budget di Zero dollari.

    Questo metodo di aggiornamento può ridurre il numero di conflitti che causano la perdita di dati, ma non può evitare la perdita di dati se vengono apportate modifiche in competizione tra loro alla stessa proprietà di un'entità. Questo funzionamento di Entity Framework dipende dalla modalità di implementazione del codice di aggiornamento. In molti casi in un'app Web questo approccio risulta poco pratico, perché richiede la gestione di grandi quantità di codice statico per tenere traccia di tutti i valori di proprietà originali per un'entità, nonché dei nuovi valori. Gestione di grandi quantità di stato può influire sulle prestazioni dell'applicazione perché richiede risorse di server oppure deve essere incluso nella pagina web stessa (ad esempio, nei campi nascosti).
- È possibile consentire la modifica di Jane sovrascrivere modifica di John. La volta successiva che un utente torna a visualizzare il reparto English, gli utenti percepiranno 8 o 8/2013 e il valore $350.000,00 ripristinato. Questo scenario è detto *Priorità client* o *Last in Wins* (Priorità ultimo accesso). (Valori del client hanno la precedenza su ciò che si trova nell'archivio dati). Come accennato nell'introduzione di questa sezione, se non si crea codice per la gestione della concorrenza questo scenario si verifica automaticamente.
- È possibile impedire modifiche di Jane venga aggiornato nel database. In genere, si potrebbe visualizzare un messaggio di errore relativa Mostra lo stato corrente dei dati e consentirle di riapplicare le modifiche se Anna vuole renderli. Questo scenario è detto *Store Wins* (Priorità archivio). I valori dell'archivio dati hanno la precedenza sui valori inoltrati dal client. In questa esercitazione viene implementato lo scenario Store Wins (Priorità archivio). Questo metodo garantisce che nessuna modifica venga sovrascritta senza che un utente riceva un avviso.

### <a name="detecting-concurrency-conflicts"></a>Il rilevamento dei conflitti di concorrenza

È possibile risolvere i conflitti gestendo [OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) le eccezioni che Entity Framework genera un'eccezione. Per determinare quando generare queste eccezioni, Entity Framework deve essere in grado di rilevare i conflitti. Pertanto è necessario configurare il database e il modello di dati in modo appropriato. Di seguito sono elencate alcune opzioni per abilitare il rilevamento dei conflitti:

- Nella tabella del database, includere una colonna di rilevamento che può essere usata per determinare quando è stata modificata una riga. È quindi possibile configurare Entity Framework per includere tale colonna i `Where` clausola SQL `Update` o `Delete` comandi.

    Il tipo di dati della colonna di rilevamento è in genere [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx). Il [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) valore è un numero sequenza che viene incrementato ogni volta che viene aggiornata la riga. In un' `Update` oppure `Delete` comando, il `Where` clausola include il valore originale della colonna di rilevamento (la versione originale). Se la riga aggiornata è stata modificata da un altro utente, il valore nel `rowversion` colonna è diverso dal valore originale, in modo che il `Update` oppure `Delete` istruzione non è possibile trovare la riga da aggiornare perché il `Where` clausola. Quando Entity Framework rileva che nessuna riga è stata aggiornata mediante la `Update` o `Delete` comando (ovvero, quando il numero di righe interessate è pari a zero), interpreta questo fatto come un conflitto di concorrenza.
- Configurare Entity Framework per includere i valori originali di ogni colonna nella tabella di `Where` della clausola `Update` e `Delete` comandi.

    Come la prima opzione, qualsiasi elemento nella riga è stato modificato dopo la riga è stato letto prima di tutto, il `Where` clausola non restituisce una riga da aggiornare, che Entity Framework interpreta come un conflitto di concorrenza. Per le tabelle di database con molte colonne, questo approccio può comportare grandi `Where` clausole e può richiede la gestione di grandi quantità di stato. Come indicato in precedenza, la gestione di grandi quantità di stato può influire sulle prestazioni dell'applicazione poiché richiede risorse di server o deve essere incluso nella pagina web stessa. Pertanto questo approccio in genere sconsigliato e non è il metodo usato in questa esercitazione.

    Se si desidera implementare questo approccio alla concorrenza, è necessario contrassegnare tutte le proprietà di chiave non primaria dell'entità che si desidera tenere traccia della concorrenza aggiungendo il [ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) attributo ad essi. Che modifica consente a Entity Framework includere tutte le colonne di SQL `WHERE` clausola di `UPDATE` istruzioni.

Nella parte restante di questa esercitazione si aggiungerà un [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) proprietà di rilevamento di `Department` entità, creare un controller e visualizzazioni e di test per verificare che tutto funzioni correttamente.

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>Aggiungere una proprietà di concorrenza ottimistica per l'entità Department

Nelle *Models\Department.cs*, aggiungere una proprietà di rilevamento denominata `RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=18-19)]

Il [Timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) attributo specifica che verrà incluse questa colonna le `Where` clausola del `Update` e `Delete` comandi inviati al database. L'attributo viene chiamato [Timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) perché le versioni precedenti di SQL Server usavano un database SQL [timestamp](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx) tipo di dati prima di SQL [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) sostituita. Il tipo .net per `rowversion` è una matrice di byte. Se si preferisce usare l'API fluent, è possibile usare la [IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx) metodo per specificare la proprietà di rilevamento, come illustrato nell'esempio seguente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

In seguito all'aggiunta di una proprietà il modello di database è stato modificato, pertanto è necessario eseguire una nuova migrazione. Nella console di Gestione pacchetti immettere i comandi seguenti:

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="create-a-department-controller"></a>Creare un Controller di reparto

Creare un `Department` controller e visualizzazioni allo stesso modo è stato fatto altri controller, usando le impostazioni seguenti:

![Add_Controller_dialog_box_for_Department_controller](https://asp.net/media/2578041/Windows-Live-Writer_Handling-C.NET-MVC-Application-7-of-10h1_AFDC_Add_Controller_dialog_box_for_Department_controller_d1d9c788-f970-4d6a-9f5a-1eddc84330b7.png)

Nelle *Controllers\DepartmentController.cs*, aggiungere un `using` istruzione:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Modifica "LastName" in "FullName" ovunque in questo file (quattro occorrenze) in modo che il reparto amministratore elenco a discesa Elenca conterrà il nome completo dell'insegnante anziché solo il cognome.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

Sostituire il codice esistente per il `HttpPost` `Edit` metodo con il codice seguente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

La vista archivierà originale `RowVersion` valore in un campo nascosto. Quando viene creato lo strumento individuerebbe il `department` istanza, quell'oggetto avranno originale `RowVersion` valore della proprietà e i nuovi valori per le altre proprietà, così come immesse dall'utente nella pagina di modifica. Quando Entity Framework crea quindi un database SQL `UPDATE` comando, comando includerà una `WHERE` clausola che esegue la ricerca di una riga con l'originale `RowVersion` valore.

Se nessuna riga è interessata dal `UPDATE` comando (nessuna riga ha originale `RowVersion` valore), Entity Framework genera una `DbUpdateConcurrencyException` eccezione e il codice nel `catch` blocco Ottiene l'oggetto interessato `Department` entità dall'eccezione oggetto. Questa entità contiene sia i valori letti dal database e i nuovi valori immessi dall'utente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Successivamente, il codice aggiunge un messaggio di errore personalizzato per ogni colonna con valori di database diversi da ciò che l'utente ha immesso nella pagina di modifica:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Un messaggio di errore più lungo spiega cosa è successo e operazioni da eseguire su di esso:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Infine, il codice imposta la `RowVersion` pari al `Department` oggetto per il nuovo valore recuperato dal database. Questo nuovo valore `RowVersion` viene archiviato nel campo nascosto quando viene visualizzata nuovamente la pagina Edit (Modifica). Quando l'utente torna a fare clic su **Salva** vengono rilevati solo gli errori di concorrenza che si verificano dopo la nuova visualizzazione della pagina Edit (Modifica).

Nelle *Views\Department\Edit.cshtml*, aggiungere un campo nascosto per salvare le `RowVersion` valore della proprietà, immediatamente dopo il campo nascosto per il `DepartmentID` proprietà:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml?highlight=17)]

Nelle *Views\Department\Index.cshtml*, sostituire il codice esistente con il codice seguente per spostare i collegamenti di riga a sinistra e modificare le intestazioni di titolo e alla colonna di pagina per visualizzare `FullName` invece di `LastName` nel **Amministratore** colonna:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]

## <a name="testing-optimistic-concurrency-handling"></a>La gestione della concorrenza ottimistica di test

Esecuzione del sito e fare clic su **reparti**:

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Fare clic il **modifica** collegamento ipertestuale per Kim Abercrombie e selezionare **aperto in una nuova scheda,** quindi fare clic sui **modifica** collegamento ipertestuale per Kim Abercrombie. Le due finestre vengono visualizzate le stesse informazioni.

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Modificare un campo nella finestra del browser prima e fare clic su **salvare**.

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Il browser visualizza la pagina Index con il valore modificato.

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Modificare qualsiasi campo nella seconda finestra del browser e fare clic su **salvare**.

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Fare clic su **salvare** nella seconda finestra del browser. Viene visualizzato un messaggio di errore:

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Fare di nuovo clic su **Salva**. Il valore che immesso nel browser secondo viene salvato insieme al valore originale dei dati modificati nel browser prima. I valori salvati vengono visualizzati nella pagina Index.

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Aggiornare la pagina Delete

Per la pagina Delete (Elimina), Entity Framework rileva conflitti di concorrenza causati da un altro utente che ha modificato il reparto con modalità simili. Quando la `HttpGet` `Delete` metodo visualizza la conferma, la visualizzazione include originale `RowVersion` valore in un campo nascosto. Questo valore viene quindi disponibile per il `HttpPost` `Delete` metodo chiamato quando l'utente conferma l'eliminazione. Quando Entity Framework crea il codice SQL `DELETE` comando, include un `WHERE` clausola con l'originale `RowVersion` valore. Se i risultati del comando nessuna riga interessata (ovvero la riga è stata modificata dopo che è stata visualizzata la pagina di conferma Delete), viene generata un'eccezione di concorrenza e il `HttpGet Delete` metodo viene chiamato con un flag di errore impostato su `true` per poter visualizzare nuovamente il pagina di conferma con un messaggio di errore. È anche possibile che siano stati interessati zero righe, perché la riga è stata eliminata da un altro utente, in modo che in questo caso viene visualizzato un messaggio di errore diversi.

Nelle *DepartmentController.cs*, sostituire il `HttpGet` `Delete` metodo con il codice seguente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

Il metodo accetta un parametro facoltativo che indica se la pagina viene nuovamente visualizzata dopo un errore di concorrenza. Se questo flag viene `true`, viene inviato un messaggio di errore alla vista usando un `ViewBag` proprietà.

Sostituire il codice nel `HttpPost` `Delete` metodo (denominato `DeleteConfirmed`) con il codice seguente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Nel codice sottoposto a scaffolding appena sostituito, questo metodo accettava solo un ID record:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

È stato modificato questo parametro su un `Department` istanza dell'entità creata dallo strumento di associazione di modelli. In questo modo è possibile accedere al `RowVersion` valore della proprietà oltre alla chiave del record.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Anche il nome del metodo di azione è stato modificato da `DeleteConfirmed` a `Delete`. Il nome in codice con scaffolding il `HttpPost` `Delete` metodo `DeleteConfirmed` per fornire la `HttpPost` metodo una firma univoca. (Common Language Runtime richiede che i metodi di overload dispongano di parametri di metodo diversi). Ora che le firme sono univoche, è possibile utilizzare la convenzione di MVC e usare lo stesso nome per il `HttpPost` e `HttpGet` metodi di eliminazione.

Se viene rilevato un errore di concorrenza, il codice visualizza nuovamente la pagina di conferma Delete (Elimina) e visualizza un flag indicante che è necessario visualizzare un messaggio di errore di concorrenza.

Nelle *Views\Department\Delete.cshtml*, sostituire il codice con scaffolding con il codice seguente che effettua alcune informazioni di formattazione viene modificato e aggiunge un campo di messaggio di errore. Le modifiche sono evidenziate.

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml?highlight=9,37,40,45-46)]

Questo codice aggiunge un messaggio di errore tra il `h2` e `h3` intestazioni:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

Sostituisce `LastName` con `FullName` nel `Administrator` campo:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Infine, aggiunge i campi nascosti per i `DepartmentID` e `RowVersion` proprietà dopo la `Html.BeginForm` istruzione:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Eseguire la pagina Departments Index. Fare clic il **eliminare** collegamento ipertestuale per il reparto English e selezionare **Apri in nuova finestra** quindi nella prima finestra fare clic sul **modifica** collegamento ipertestuale per la lingua inglese reparto.

Nella prima finestra modificare uno dei valori e fare clic su **salvare** :

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

La pagina di indice viene confermata la modifica.

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

Nella seconda finestra, fare clic su **Elimina**.

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

Viene visualizzato il messaggio di errore di concorrenza e i valori di Department (Reparto) vengono aggiornati con i dati attualmente presenti nel database.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Se si fa di nuovo clic su **Delete** (Elimina) viene visualizzata la pagina Index che indica che il reparto è stato eliminato.

## <a name="summary"></a>Riepilogo

Questo argomento completa l'introduzione alla gestione dei conflitti di concorrenza. Per informazioni su altri modi per gestire diversi scenari di concorrenza, vedere [modelli di concorrenza ottimistica](https://blogs.msdn.com/b/adonet/archive/2011/02/03/using-dbcontext-in-ef-feature-ctp5-part-9-optimistic-concurrency-patterns.aspx) e [utilizzano i valori di proprietà](https://blogs.msdn.com/b/adonet/archive/2011/01/30/using-dbcontext-in-ef-feature-ctp5-part-5-working-with-property-values.aspx) nel blog del team di Entity Framework. L'esercitazione successiva illustra come implementare l'ereditarietà tabella per gerarchia per il `Instructor` e `Student` entità.

Sono disponibili collegamenti ad altre risorse di Entity Framework nel [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Precedente](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Successivo](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
