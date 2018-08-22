---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
title: Introduzione a Entity Framework 4.0 Database First e ASP.NET 4 Web Form - parte 3 | Microsoft Docs
author: tdykstra
description: L'applicazione web di esempio Contoso University illustra come creare applicazioni Web Form ASP.NET utilizzando Entity Framework. L'applicazione di esempio è...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: ccdc3f8c-2568-40a7-8f8b-3c23d2e05388
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
msc.type: authoredcontent
ms.openlocfilehash: 162f93c0249d8fa11d67ea10c68bc2ca8bae7259
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826755"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-3"></a>Introduzione a Entity Framework 4.0 Database First e ASP.NET 4 Web Form - parte 3
====================
da [Tom Dykstra](https://github.com/tdykstra)

> L'applicazione web di esempio Contoso University illustra come creare applicazioni Web Form ASP.NET usando il Entity Framework 4.0 e Visual Studio 2010. Per informazioni sulla serie di esercitazioni, vedere [la prima esercitazione della serie](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="filtering-ordering-and-grouping-data"></a>Filtro, ordinamento e raggruppamento dei dati

Nell'esercitazione precedente è stato usato il `EntityDataSource` controllo per visualizzare e modificare i dati. In questa esercitazione si sarà filtrare, ordinare e raggruppare i dati. Quando si esegue questa operazione impostando le proprietà del `EntityDataSource` (controllo), la sintassi è diversa da altri controlli origine dati. Come si vedrà, tuttavia, è possibile usare il `QueryExtender` controllo per ridurre al minimo queste differenze.

Sarà necessario modificare il *Students.aspx* pagina per filtrare per studenti, Ordina per nome e ricerca sul nome. Sarà anche necessario modificare il *Courses.aspx* pagina per visualizzare i corsi per il dipartimento selezionato e la ricerca per i corsi in base al nome. Infine, si aggiungerà le statistiche degli studenti per la *About* pagina.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image1.png)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image3.png)

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image5.png)

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image7.png)

## <a name="using-the-entitydatasource-where-property-to-filter-data"></a>Utilizzo di EntityDataSource "Where" proprietà per filtrare i dati

Aprire il *Students.aspx* pagina creata nell'esercitazione precedente. Come attualmente configurata, il `GridView` controllo nella pagina vengono visualizzati tutti i nomi di `People` set di entità. Tuttavia, si desidera visualizzare solo gli studenti, che è possibile trovare selezionando `Person` le entità con le date di iscrizione non null.

Passare a **Design** consente di visualizzare e selezionare il `EntityDataSource` controllo. Nel **le proprietà** impostare nella finestra di `Where` proprietà `it.EnrollmentDate is not null`.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-3/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image9.png)

La sintassi da utilizzare nel `Where` proprietà del `EntityDataSource` controllo è Entity SQL. Entity SQL è simile a Transact-SQL, ma è stato personalizzato per l'uso con le entità anziché gli oggetti di database. Nell'espressione `it.EnrollmentDate is not null`, la parola `it` rappresenta un riferimento all'entità restituite dalla query. Pertanto `it.EnrollmentDate` fa riferimento al `EnrollmentDate` proprietà delle `Person` entità che il `EntityDataSource` controllare restituisce.

Eseguire la pagina. L'elenco di studenti contiene ora solo gli studenti. (Non sono presenti righe visualizzati in cui nessuna data di registrazione.)

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image11.png)

## <a name="using-the-entitydatasource-orderby-property-to-order-data"></a>Usando la proprietà di EntityDataSource "OrderBy" per i dati degli ordini

È possibile che sia in ordine di nome quando viene visualizzato prima di tutto questo elenco. Con il *Students.aspx* ancora aperta nel **progettazione** visualizzazione e con il `EntityDataSource` controllo ancora selezionato, nella **proprietà** set finestra il  **OrderBy** proprietà `it.LastName`.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-3/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image13.png)

Eseguire la pagina. Elenco degli studenti è ora nell'ordine in base al cognome.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-3/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image15.png)

## <a name="using-a-control-parameter-to-set-the-where-property"></a>Utilizzo di un parametro di controllo per impostare la proprietà "Where"

Come con altri controlli origine dati, è possibile passare i valori dei parametri di `Where` proprietà. Nel *Courses.aspx* pagina che è stato creato nella parte 2 dell'esercitazione, è possibile utilizzare questo metodo per visualizzare i corsi associati con il reparto che un utente seleziona dall'elenco a discesa.

Aprire *Courses.aspx* e passare alla **progettazione** visualizzazione. Aggiungere una seconda `EntityDataSource` alla pagina di controllo e denominarlo `CoursesEntityDataSource`. Connetterlo al `SchoolEntities` del modello e selezionare `Courses` come la **EntitySetName** valore.

Nel **delle proprietà** finestra, fare clic sui puntini di sospensione nel **in cui** finestra delle proprietà. (Assicurarsi che il `CoursesEntityDataSource` controllo sia ancora selezionato prima di usare il **proprietà** finestra.)

[![Image06](the-entity-framework-and-aspnet-getting-started-part-3/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image17.png)

Il **Editor di espressioni** verrà visualizzata la finestra di dialogo. Nella finestra di dialogo, selezionare **generare automaticamente Where espressione in base ai parametri forniti**, quindi fare clic su **Aggiungi parametro**. Il parametro `DepartmentID`, selezionare **controllo** come il **parametro source** valore, quindi selezionare **DepartmentsDropDownList** come il **ControlID**  valore.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-3/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image19.png)

Fare clic su **Mostra proprietà avanzate**e nel **proprietà** finestra di **Editor espressioni** nella finestra di dialogo Modifica il `Type` proprietà `Int32`.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-3/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image21.png)

Al termine, fare clic su **OK**.

Sotto l'elenco a discesa, aggiungere un `GridView` alla pagina di controllo e denominarlo `CoursesGridView`. Connetterlo al `CoursesEntityDataSource` controllo origine dati, fare clic su **Aggiorna Schema**, fare clic su **Modifica colonne**e rimuovere il `DepartmentID` colonna. Il `GridView` markup del controllo è simile al seguente.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample1.aspx)]

Quando l'utente modifica il reparto selezionato nell'elenco a discesa, è necessario l'elenco dei corsi associati cambiare automaticamente. Rendere questo risultato, selezionare l'elenco a discesa e nel **delle proprietà** set finestra il `AutoPostBack` proprietà `True`.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-3/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image23.png)

Ora che è più necessario usare la finestra di progettazione, passare a **origine** consente di visualizzare e sostituire il `ConnectionString` e `DefaultContainer` denominano le proprietà del `CoursesEntityDataSource` controllare con il `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` attributo. Al termine, il markup per il controllo avrà un aspetto simile all'esempio seguente.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample2.aspx)]

Eseguire la pagina e usare l'elenco a discesa per selezionare diversi reparti. Solo i corsi offerti dal reparto selezionato vengono visualizzati di `GridView` controllo.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-3/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image25.png)

## <a name="using-the-entitydatasource-groupby-property-to-group-data"></a>Usando la proprietà "GroupBy" EntityDataSource per raggruppare i dati

Si supponga che Contoso University voglia inserire alcune statistiche degli studenti-body nella relativa pagina di informazioni. In particolare, desidera Mostra una suddivisione di un numero di studenti per la data che sono registrati.

Aprire *About*e nella **origine** consente di visualizzare, sostituire il contenuto esistente del `BodyContent` controllare con "Le statistiche degli studenti corpo" tra `h2` tag:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample3.aspx)]

Dopo l'intestazione, aggiungere un' `EntityDataSource` controllano e denominarlo `StudentStatisticsEntityDataSource`. Connetterlo al `SchoolEntities`, selezionare il `People` entità impostata e lasciare il **seleziona** finestra della procedura guidata non modificato. Impostare le proprietà seguenti **proprietà** finestra:

- Per filtrare solo per gli studenti, impostare il `Where` proprietà `it.EnrollmentDate is not null`.
- Per raggruppare i risultati in base alla data di registrazione, impostare il `GroupBy` proprietà `it.EnrollmentDate`.
- Per selezionare la data di registrazione e il numero di studenti, impostare il `Select` proprietà `it.EnrollmentDate, Count(it.EnrollmentDate) AS NumberOfStudents`.
- Per ordinare i risultati per la data di registrazione, impostare il `OrderBy` proprietà `it.EnrollmentDate`.

Nelle **origine** consente di visualizzare, sostituire il `ConnectionString` e `DefaultContainer` denominano le proprietà con un `ContextTypeName` proprietà. Il `EntityDataSource` markup del controllo è ora simile al seguente.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample4.aspx)]

La sintassi del `Select`, `GroupBy`, e `Where` delle proprietà è simile a Transact-SQL, ad eccezione del `it` (parola chiave) che specifica l'entità corrente.

Aggiungere il markup seguente per creare un `GridView` controllo per visualizzare i dati.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample5.aspx)]

Eseguire la pagina per visualizzare un elenco che mostra il numero di studenti per data di registrazione.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image27.png)

## <a name="using-the-queryextender-control-for-filtering-and-ordering"></a>Tramite il controllo QueryExtender per il filtro e ordinamento

Il `QueryExtender` controllo fornisce un modo per specificare il filtro e ordinamento nel markup. La sintassi è indipendente dal sistema di gestione di database (DBMS) in uso. È inoltre in genere indipendente da Entity Framework, con l'eccezione che la sintassi utilizzata per le proprietà di navigazione sia univoca a Entity Framework.

In questa parte dell'esercitazione si userà un `QueryExtender` controllo ai dati di filtro e l'ordine e uno dei campi di order by sarà una proprietà di navigazione.

(Se si preferisce usare codice invece di markup per estendere le query che vengono generate automaticamente mediante il `EntityDataSource` (controllo), è possibile farlo gestione di `QueryCreated` evento. Questo è il `QueryExtender` controllo si estende `EntityDataSource` controllano anche le query.)

Aprire il *Courses.aspx* pagina e sotto il codice aggiunto in precedenza, inserire il markup seguente per creare un titolo, una casella di testo per l'immissione di stringhe di ricerca, un pulsante di ricerca e un `EntityDataSource` controllo a cui è associato il `Courses` set di entità.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample6.aspx)]

Si noti che il `EntityDataSource` del controllo `Include` è impostata su `Department`. Nel database, il `Course` tabella non contiene il nome del reparto; contiene un `DepartmentID` colonna chiave esterna. Se si sono stati la query direttamente, il database per ottenere il nome del reparto insieme ai dati del corso, sarebbe necessario aggiungere il `Course` e `Department` tabelle. Impostando il `Include` proprietà `Department`, si specifica che Entity Framework deve eseguire le operazioni di recupero correlato `Department` entità quando raggiunge un `Course` entità. Il `Department` entità verrà quindi archiviato nel `Department` proprietà di navigazione del `Course` entità. (Per impostazione predefinita, il `SchoolEntities` classe che è stato generato da Progettazione modelli dati recupera i dati correlati quando necessario e il controllo origine dati è stato associato a tale classe, pertanto l'impostazione di `Include` proprietà non è necessaria. Tuttavia, impostandola migliora le prestazioni della pagina, perché in caso contrario, Entity Framework renderebbe separare le chiamate al database per recuperare i dati per il `Course` entità e per i relativi `Department` entities.)

Dopo il `EntityDataSource` controllo appena creato, inserire il markup seguente per creare un `QueryExtender` controllo associato a quella `EntityDataSource` controllo.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample7.aspx)]

Il `SearchExpression` elemento specifica che si desidera selezionare i corsi cui titolo corrisponde con il valore immesso nella casella di testo. Solo come numero di caratteri immessi nella casella di testo verrà confrontati, perché il `SearchType` proprietà specifica `StartsWith`.

Il `OrderByExpression` elemento specifica che il set di risultati verrà ordinato titolo del corso nel nome del reparto. Si noti come viene specificato il nome di reparto: `Department.Name`. Poiché l'associazione tra il `Course` entità e il `Department` entità è uno a uno, il `Department` contiene proprietà di navigazione un `Department` entità. (Se si trattasse di una relazione uno-a-molti, la proprietà conterrà una raccolta.) Per ottenere il nome del reparto, è necessario specificare il `Name` proprietà del `Department` entità.

Aggiungere infine un `GridView` controllo per visualizzare l'elenco dei corsi:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample8.aspx)]

La prima colonna è un campo di modello che visualizza il nome del reparto. Specifica l'espressione di Data Binding `Department.Name`, esattamente come si è visto nella `QueryExtender` controllo.

Eseguire la pagina. La visualizzazione iniziale mostra un elenco di tutti i corsi in ordine dal reparto e quindi dal titolo del corso.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image29.png)

Immettere una "m" e fare clic su **ricerca** per visualizzare tutti i corsi cui titoli iniziano con "m" (la ricerca è non distinzione maiuscole/minuscole).

[![Image12](the-entity-framework-and-aspnet-getting-started-part-3/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image31.png)

## <a name="using-the-like-operator-to-filter-data"></a>Utilizzo dell'operatore "Like" per filtrare i dati

È possibile ottenere un effetto simile al `QueryExtender` del controllo `StartsWith`, `Contains`, e `EndsWith` cercare tipi utilizzando una `Like` operatore nel `EntityDataSource` del controllo `Where` proprietà. In questa parte dell'esercitazione, verrà illustrato come usare il `Like` operatore per la ricerca di uno studente in base al nome.

Aprire *Students.aspx* nelle **origine** visualizzazione. Dopo il `GridView` di controllo, aggiungere il markup seguente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample9.aspx)]

Questo markup è simile a ciò che si è visto in precedenza, ad eccezione del `Where` valore della proprietà. Nella seconda parte i `Where` espressione definisce ricerca di una sottostringa (`LIKE %FirstMidName% or LIKE %LastName%`) che cerca i nomi e il cognome per ciò che viene digitato nella casella di testo.

Eseguire la pagina. Inizialmente è possibile visualizzare tutti gli studenti perché il valore predefinito per il `StudentName` parametro è "%".

[![Image13](the-entity-framework-and-aspnet-getting-started-part-3/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image33.png)

Immettere la lettera "g" nella casella di testo e fare clic su **ricerca**. Viene visualizzato un elenco di studenti che hanno un "g" in entrambi il nome o cognome.

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image35.png)

È stata visualizzata, aggiornati, filtrato, ordinati ed raggruppati i dati da singole tabelle. Nella prossima esercitazione inizierai a lavorare con i dati correlati (scenari di master-dettagli).

> [!div class="step-by-step"]
> [Precedente](the-entity-framework-and-aspnet-getting-started-part-2.md)
> [Successivo](the-entity-framework-and-aspnet-getting-started-part-4.md)
