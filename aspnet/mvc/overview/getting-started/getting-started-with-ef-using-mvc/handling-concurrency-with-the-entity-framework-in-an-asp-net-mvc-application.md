---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: Gestione della concorrenza con Entity Framework 6 in un'applicazione ASP.NET MVC 5 (10 pari a 12) | Microsoft Docs
author: tdykstra
description: L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 5 con Entity Framework 6 Code First e Visual Studio...
ms.author: riande
ms.date: 12/08/2014
ms.assetid: be0c098a-1fb2-457e-b815-ddca601afc65
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 708c38b8e0815c1d8b899c4d5a6f878e235340bc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823927"
---
<a name="handling-concurrency-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-10-of-12"></a>Gestione della concorrenza con Entity Framework 6 in un'applicazione ASP.NET MVC 5 (10 pari a 12)
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto completato](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) o [Scarica il PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 5 con Entity Framework 6 Code First e Visual Studio 2013. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Nelle esercitazioni precedenti è stato illustrato come aggiornare i dati. Questa esercitazione descrive la gestione dei conflitti quando più utenti aggiornano la stessa entità contemporaneamente.

Sarà necessario modificare le pagine web che funzionano con la `Department` entità in modo che consentono di gestire gli errori di concorrenza. Le illustrazioni seguenti mostrano le pagine di indice e Delete, inclusi alcuni messaggi che vengono visualizzati se si verifica un conflitto di concorrenza.

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>Conflitti di concorrenza

Un conflitto di concorrenza si verifica quando un utente visualizza i dati di un'entità per modificarli mentre un altro utente aggiorna i dati della stessa entità prima che la modifica del primo utente venga scritta nel database. Se non si abilita il rilevamento di questi conflitti, l'ultimo utente che aggiorna il database sovrascrive le modifiche apportate dall'altro utente. In molte applicazioni questo rischio è accettabile: se il numero di utenti è ridotto o se l'eventuale sovrascrittura di alcune modifiche non è un aspetto critico, i costi della programmazione per la concorrenza possono superare i vantaggi. In tal caso non è necessario configurare l'applicazione per la gestione dei conflitti di concorrenza.

### <a name="pessimistic-concurrency-locking"></a>Concorrenza pessimistica (blocco)

Se è importante che l'applicazione eviti la perdita accidentale di dati in scenari di concorrenza, un metodo per garantire che ciò accada è l'uso dei blocchi di database. Questa operazione viene definita *concorrenza pessimistica*. Ad esempio, prima di leggere una riga da un database si richiede un blocco per l'accesso di sola lettura o per l'accesso in modalità aggiornamento. Se si blocca una riga per l'accesso di aggiornamento, nessun altro utente può bloccare la riga per l'accesso di sola lettura o di aggiornamento, perché riceverebbe una copia di dati dei quali è in corso la modifica. Se si blocca una riga per l'accesso in sola lettura, anche altri utenti possono bloccarla per l'accesso in sola lettura, ma non per l'aggiornamento.

La gestione dei blocchi presenta svantaggi. La sua programmazione può risultare complicata. Richiede molte risorse di gestione del database e può causare problemi di prestazioni quando il numero di utenti di un'applicazione aumenta. Per questi motivi non tutti i sistemi di gestione di database supportano la concorrenza pessimistica. Entity Framework non fornisce alcun supporto predefinito per questa e in questa esercitazione non illustra come implementarla.

### <a name="optimistic-concurrency"></a>Concorrenza ottimistica

È l'alternativa alla concorrenza pessimistica *la concorrenza ottimistica*. Nella concorrenza ottimistica si consente che i conflitti di concorrenza si verifichino, quindi si reagisce con le modalità appropriate. Ad esempio, Giorgio esegue la pagina di modifica di reparti, le modifiche il **Budget** quantità per il reparto English da $350.000,00 a $0,00.

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Prima di John fa clic su **salvare**, Jane esegue la stessa pagina e le modifiche le **Start Date** 9/1/2007 campo a 8 o 8/2013.

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

John fa clic su **salvare** prima e vede sua modifica quando il browser torna alla pagina di indice, Jane quindi fa clic su **salvare**. Le operazioni successive dipendono da come si decide di gestire i conflitti di concorrenza. Di seguito sono elencate alcune opzioni:

- È possibile tenere traccia della proprietà che un utente ha modificato e aggiornare solo le colonne corrispondenti nel database. Nello scenario dell'esempio non si perde nessun dato, perché i due utenti hanno aggiornato proprietà diverse. La volta successiva che un utente torna a visualizzare il reparto English, verranno visualizzate le modifiche sia di John e Jane, ovvero una data di inizio di 8 o 8/2013 e un budget di Zero dollari.

    Questo metodo di aggiornamento può ridurre il numero di conflitti che causano la perdita di dati, ma non può evitare la perdita di dati se vengono apportate modifiche in competizione tra loro alla stessa proprietà di un'entità. Questo funzionamento di Entity Framework dipende dalla modalità di implementazione del codice di aggiornamento. In molti casi in un'app Web questo approccio risulta poco pratico, perché richiede la gestione di grandi quantità di codice statico per tenere traccia di tutti i valori di proprietà originali per un'entità, nonché dei nuovi valori. La gestione di grandi quantità di codice statico può influire sulle prestazioni dell'applicazione, perché richiede risorse di server o deve essere inclusa nella pagina Web stessa (ad esempio in campi nascosti) o in un cookie.
- È possibile consentire la modifica di Jane sovrascrivere modifica di John. La volta successiva che un utente torna a visualizzare il reparto English, gli utenti percepiranno 8 o 8/2013 e il valore $350.000,00 ripristinato. Questo scenario è detto *Priorità client* o *Last in Wins* (Priorità ultimo accesso). Tutti i valori del client hanno la precedenza sui valori presenti nell'archivio dati. Come accennato nell'introduzione di questa sezione, se non si crea codice per la gestione della concorrenza questo scenario si verifica automaticamente.
- È possibile impedire modifiche di Jane venga aggiornato nel database. In genere, si potrebbe visualizzare un messaggio di errore relativa Mostra lo stato corrente dei dati e consentirle di riapplicare le modifiche se Anna vuole renderli. Questo scenario è detto *Store Wins* (Priorità archivio). I valori dell'archivio dati hanno la precedenza sui valori inoltrati dal client. In questa esercitazione viene implementato lo scenario Store Wins (Priorità archivio). Questo metodo garantisce che nessuna modifica venga sovrascritta senza che un utente riceva un avviso.

### <a name="detecting-concurrency-conflicts"></a>Il rilevamento dei conflitti di concorrenza

È possibile risolvere i conflitti gestendo [OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) le eccezioni che Entity Framework genera un'eccezione. Per determinare quando generare queste eccezioni, Entity Framework deve essere in grado di rilevare i conflitti. Pertanto è necessario configurare il database e il modello di dati in modo appropriato. Di seguito sono elencate alcune opzioni per abilitare il rilevamento dei conflitti:

- Nella tabella del database, includere una colonna di rilevamento che può essere usata per determinare quando è stata modificata una riga. È quindi possibile configurare Entity Framework per includere tale colonna i `Where` clausola SQL `Update` o `Delete` comandi.

    Il tipo di dati della colonna di rilevamento è in genere [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx). Il [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) valore è un numero sequenza che viene incrementato ogni volta che viene aggiornata la riga. In un' `Update` oppure `Delete` comando, il `Where` clausola include il valore originale della colonna di rilevamento (la versione originale). Se la riga aggiornata è stata modificata da un altro utente, il valore nel `rowversion` colonna è diverso dal valore originale, in modo che il `Update` oppure `Delete` istruzione non è possibile trovare la riga da aggiornare perché il `Where` clausola. Quando Entity Framework rileva che nessuna riga è stata aggiornata mediante la `Update` o `Delete` comando (ovvero, quando il numero di righe interessate è pari a zero), interpreta questo fatto come un conflitto di concorrenza.
- Configurare Entity Framework per includere i valori originali di ogni colonna nella tabella di `Where` della clausola `Update` e `Delete` comandi.

    Come la prima opzione, qualsiasi elemento nella riga è stato modificato dopo la riga è stato letto prima di tutto, il `Where` clausola non restituisce una riga da aggiornare, che Entity Framework interpreta come un conflitto di concorrenza. Per le tabelle di database con molte colonne, questo approccio può comportare grandi `Where` clausole e può richiede la gestione di grandi quantità di stato. Come notato in precedenza, la gestione di grandi quantità di codice statico può ridurre le prestazioni dell'applicazione. Pertanto questo approccio è in genere sconsigliato e non è il metodo usato in questa esercitazione.

    Se si desidera implementare questo approccio alla concorrenza, è necessario contrassegnare tutte le proprietà di chiave non primaria dell'entità che si desidera tenere traccia della concorrenza aggiungendo il [ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) attributo ad essi. Che modifica consente a Entity Framework includere tutte le colonne di SQL `WHERE` clausola di `UPDATE` istruzioni.

Nella parte restante di questa esercitazione si aggiungerà un [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) proprietà di rilevamento di `Department` entità, creare un controller e visualizzazioni e di test per verificare che tutto funzioni correttamente.

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>Aggiungere una proprietà di concorrenza ottimistica per l'entità Department

Nelle *Models\Department.cs*, aggiungere una proprietà di rilevamento denominata `RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=20-22)]

Il [Timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) attributo specifica che verrà incluse questa colonna le `Where` clausola del `Update` e `Delete` comandi inviati al database. L'attributo viene chiamato [Timestamp](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) perché le versioni precedenti di SQL Server usavano un database SQL [timestamp](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx) tipo di dati prima di SQL [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) sostituita. Il tipo .net per [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) è una matrice di byte.

Se si preferisce usare l'API fluent, è possibile usare la [IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx) metodo per specificare la proprietà di rilevamento, come illustrato nell'esempio seguente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

In seguito all'aggiunta di una proprietà il modello di database è stato modificato, pertanto è necessario eseguire una nuova migrazione. Nella console di Gestione pacchetti immettere i comandi seguenti:

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="modify-the-department-controller"></a>Modificare il Controller Department

Nelle *Controllers\DepartmentController.cs*, aggiungere un `using` istruzione:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Nel *DepartmentController.cs* file, modificare tutte le occorrenze di "LastName" in "FullName" in modo che gli elenchi di elenco a discesa di amministratore di reparto conterrà il nome completo dell'insegnante anziché solo il cognome.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

Sostituire il codice esistente per il `HttpPost` `Edit` metodo con il codice seguente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Se il metodo `FindAsync` restituisce null, il reparto è stato eliminato da un altro utente. Il codice illustrato Usa i valori di modulo per creare un'entità reparto, in modo che la pagina di modifica può essere visualizzata nuovamente con un messaggio di errore. In alternativa, non è necessario creare nuovamente l'entità del reparto se si visualizza solo un messaggio di errore senza visualizzare di nuovo i campi del reparto.

La visualizzazione archivia originale `RowVersion` valore in un campo nascosto e il metodo lo riceve nel `rowVersion` parametro. Prima della chiamata di `SaveChanges` è necessario inserire il valore originale della proprietà `RowVersion` nella raccolta `OriginalValues` dell'entità. Quando Entity Framework crea quindi un database SQL `UPDATE` comando, comando includerà una `WHERE` clausola che esegue la ricerca di una riga con l'originale `RowVersion` valore.

Se nessuna riga è interessata dal `UPDATE` comando (nessuna riga ha originale `RowVersion` valore), Entity Framework genera una `DbUpdateConcurrencyException` eccezione e il codice nel `catch` blocco Ottiene l'oggetto interessato `Department` entità dall'eccezione oggetto.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Questo oggetto contiene i nuovi valori immessi dall'utente nel relativo `Entity` proprietà ed è possibile ottenere i valori letti dal database chiamando la `GetDatabaseValues` (metodo).

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Il `GetDatabaseValues` metodo restituisce null se un utente ha eliminato la riga dal database; in caso contrario, è necessario eseguire il cast dell'oggetto restituito al `Department` classe per poter accedere il `Department` proprietà. (Perché già selezionata per l'eliminazione, `databaseEntry` sarebbe null solo se il reparto è stato eliminato dopo `FindAsync` viene eseguita e prima `SaveChanges` viene eseguito.)

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Successivamente, il codice aggiunge un messaggio di errore personalizzato per ogni colonna con valori di database diversi da ciò che l'utente ha immesso nella pagina di modifica:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Un messaggio di errore più lungo spiega cosa è successo e operazioni da eseguire su di esso:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Infine, il codice imposta la `RowVersion` pari al `Department` oggetto per il nuovo valore recuperato dal database. Questo nuovo valore `RowVersion` viene archiviato nel campo nascosto quando viene visualizzata nuovamente la pagina Edit (Modifica). Quando l'utente torna a fare clic su **Salva** vengono rilevati solo gli errori di concorrenza che si verificano dopo la nuova visualizzazione della pagina Edit (Modifica).

Nelle *Views\Department\Edit.cshtml*, aggiungere un campo nascosto per salvare le `RowVersion` valore della proprietà, immediatamente dopo il campo nascosto per il `DepartmentID` proprietà:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=18)]

## <a name="testing-optimistic-concurrency-handling"></a>La gestione della concorrenza ottimistica di test

Esecuzione del sito e fare clic su **reparti**:

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Fare clic il **modifica** collegamento ipertestuale per il reparto English e selezionare **aperto in una nuova scheda,** quindi fare clic sui **modifica** collegamento ipertestuale per il reparto English. Le due schede visualizzano le stesse informazioni.

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Modificare un campo nella prima scheda del browser e fare clic su **Salva**.

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Il browser visualizza la pagina Index con il valore modificato.

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Modificare un campo nella seconda scheda del browser e fare clic su **salvare**.

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Fare clic su **salvare** nella seconda scheda del browser. Viene visualizzato un messaggio di errore:

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Fare di nuovo clic su **Salva**. Il valore che immesso nella seconda scheda del browser viene salvato insieme al valore originale dei dati che è stato modificato nel browser prima. I valori salvati vengono visualizzati nella pagina Index.

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Aggiornare la pagina Delete

Per la pagina Delete (Elimina), Entity Framework rileva conflitti di concorrenza causati da un altro utente che ha modificato il reparto con modalità simili. Quando la `HttpGet` `Delete` metodo visualizza la conferma, la visualizzazione include originale `RowVersion` valore in un campo nascosto. Questo valore viene quindi disponibile per il `HttpPost` `Delete` metodo chiamato quando l'utente conferma l'eliminazione. Quando Entity Framework crea il codice SQL `DELETE` comando, include un `WHERE` clausola con l'originale `RowVersion` valore. Se i risultati del comando nessuna riga interessata (ovvero la riga è stata modificata dopo che è stata visualizzata la pagina di conferma Delete), viene generata un'eccezione di concorrenza e il `HttpGet Delete` metodo viene chiamato con un flag di errore impostato su `true` per poter visualizzare nuovamente il pagina di conferma con un messaggio di errore. È anche possibile che siano stati interessati zero righe, perché la riga è stata eliminata da un altro utente, in modo che in questo caso viene visualizzato un messaggio di errore diversi.

Nelle *DepartmentController.cs*, sostituire il `HttpGet` `Delete` metodo con il codice seguente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Il metodo accetta un parametro facoltativo che indica se la pagina viene nuovamente visualizzata dopo un errore di concorrenza. Se questo flag viene `true`, viene inviato un messaggio di errore alla vista usando un `ViewBag` proprietà.

Sostituire il codice nel `HttpPost` `Delete` metodo (denominato `DeleteConfirmed`) con il codice seguente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

Nel codice sottoposto a scaffolding appena sostituito, questo metodo accettava solo un ID record:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

È stato modificato questo parametro su un `Department` istanza dell'entità creata dallo strumento di associazione di modelli. In questo modo è possibile accedere al `RowVersion` valore della proprietà oltre alla chiave del record.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Anche il nome del metodo di azione è stato modificato da `DeleteConfirmed` a `Delete`. Il nome in codice con scaffolding il `HttpPost` `Delete` metodo `DeleteConfirmed` per fornire la `HttpPost` metodo una firma univoca. (Il CLR richiede metodi di overload per avere parametri di metodo diversi). Ora che le firme sono univoche, è possibile utilizzare la convenzione di MVC e usare lo stesso nome per il `HttpPost` e `HttpGet` metodi di eliminazione.

Se viene rilevato un errore di concorrenza, il codice visualizza nuovamente la pagina di conferma Delete (Elimina) e visualizza un flag indicante che è necessario visualizzare un messaggio di errore di concorrenza.

Nelle *Views\Department\Delete.cshtml*, sostituire il codice con scaffolding con il codice seguente che aggiunge un campo di messaggio di errore e i campi nascosti per le proprietà DepartmentID e RowVersion. Le modifiche sono evidenziate.

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml?highlight=9-10,21,52-54)]

Questo codice aggiunge un messaggio di errore tra il `h2` e `h3` intestazioni:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Sostituisce `LastName` con `FullName` nel `Administrator` campo:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Infine, aggiunge i campi nascosti per i `DepartmentID` e `RowVersion` proprietà dopo la `Html.BeginForm` istruzione:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cshtml)]

Eseguire la pagina Departments Index. Fare clic il **eliminare** collegamento ipertestuale per il reparto English e selezionare **aperto in una nuova scheda,** quindi nella prima scheda fare clic sui **modifica** collegamento ipertestuale per il reparto English.

Nella prima finestra modificare uno dei valori e fare clic su **salvare** :

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

La pagina di indice viene confermata la modifica.

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

Nella seconda scheda fare clic su **Delete** (Elimina).

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

Viene visualizzato il messaggio di errore di concorrenza e i valori di Department (Reparto) vengono aggiornati con i dati attualmente presenti nel database.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Se si fa di nuovo clic su **Delete** (Elimina) viene visualizzata la pagina Index che indica che il reparto è stato eliminato.

## <a name="summary"></a>Riepilogo

Questo argomento completa l'introduzione alla gestione dei conflitti di concorrenza. Per informazioni su altri modi per gestire diversi scenari di concorrenza, vedere [modelli di concorrenza ottimistica](https://msdn.microsoft.com/data/jj592904) e [utilizzano i valori di proprietà](https://msdn.microsoft.com/data/jj592677) su MSDN. L'esercitazione successiva illustra come implementare l'ereditarietà tabella per gerarchia per il `Instructor` e `Student` entità.

Sono disponibili collegamenti ad altre risorse di Entity Framework nel [l'accesso ai dati ASP.NET - risorse consigliate](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Precedente](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Successivo](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
