---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
title: Guida introduttiva a Database di Entity Framework 4.0 prima di tutto e ASP.NET 4 di Web Form - parte 3 | Documenti Microsoft
author: tdykstra
description: L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni Web Form ASP.NET tramite Entity Framework. È l'applicazione di esempio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ccdc3f8c-2568-40a7-8f8b-3c23d2e05388
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
msc.type: authoredcontent
ms.openlocfilehash: 654f3556af5d05ec186e1811421966bbaffd2e21
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-3"></a>Guida introduttiva a Database di Entity Framework 4.0 prima di tutto e form ASP.NET Web 4 - parte 3
====================
da [Tom Dykstra](https://github.com/tdykstra)

> L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni Web Form ASP.NET utilizzando il Entity Framework 4.0 e Visual Studio 2010. Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="filtering-ordering-and-grouping-data"></a>Filtro, ordinamento e raggruppamento di dati

Nell'esercitazione precedente è stata usata la `EntityDataSource` controllo per visualizzare e modificare i dati. In questa esercitazione verrà filtrare, ordinare e raggruppare i dati. Quando si esegue questa operazione impostando le proprietà del `EntityDataSource` controllo, la sintassi è diversa da altri controlli origine dati. Come si vedrà, tuttavia, è possibile utilizzare il `QueryExtender` controllo per ridurre al minimo queste differenze.

Si modificherà il *Students.aspx* pagina per filtrare per studenti, Ordina per nome e la ricerca nome. Sarà inoltre necessario impostare il *Courses.aspx* pagina per visualizzare i corsi per il reparto selezionato e la ricerca per i corsi per nome. Infine, si aggiungeranno statistiche studente per il *About* pagina.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image1.png)

[![image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image3.png)

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image5.png)

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image7.png)

## <a name="using-the-entitydatasource-where-property-to-filter-data"></a>Proprietà di EntityDataSource "Where" per filtrare i dati

Aprire il *Students.aspx* pagina in cui è stato creato nell'esercitazione precedente. Attualmente è configurato, il `GridView` controllo nella pagina vengono visualizzati tutti i nomi di `People` set di entità. Tuttavia, si desidera mostrare solo gli studenti, che è possibile trovare selezionando `Person` entità che presentano le date di registrazione non null.

Passare a **progettazione** consente di visualizzare e selezionare il `EntityDataSource` controllo. Nel **proprietà** finestra, impostare il `Where` proprietà `it.EnrollmentDate is not null`.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-3/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image9.png)

La sintassi da utilizzare nel `Where` proprietà del `EntityDataSource` controllo è Entity SQL. Entity SQL è simile a Transact-SQL, ma è stato personalizzato per l'utilizzo con entità anziché in oggetti di database. Nell'espressione `it.EnrollmentDate is not null`, la parola `it` rappresenta un riferimento all'entità restituite dalla query. Pertanto, `it.EnrollmentDate` fa riferimento al `EnrollmentDate` proprietà del `Person` entità che la `EntityDataSource` controllare restituisce.

Eseguire la pagina. L'elenco di studenti ora contiene solo gli studenti. (Non sono presenti righe visualizzati in nessuna data di registrazione.)

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image11.png)

## <a name="using-the-entitydatasource-orderby-property-to-order-data"></a>Utilizzando la proprietà di EntityDataSource "OrderBy" per ordinare dati

È anche opportuno sia nell'ordine dei nomi quando la prima visualizzazione elenco. Con il *Students.aspx* pagina ancora aperto in **progettazione** Vista e con il `EntityDataSource` controllo ancora selezionato, nel **proprietà** finestra set di  **OrderBy** proprietà `it.LastName`.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-3/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image13.png)

Eseguire la pagina. L'elenco di studenti è ora nell'ordine in base al cognome.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-3/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image15.png)

## <a name="using-a-control-parameter-to-set-the-where-property"></a>Utilizzo di un parametro di controllo per impostare la proprietà "Where"

Come con altri controlli origine dati, è possibile passare i valori dei parametri di `Where` proprietà. Nel *Courses.aspx* pagina creato nella parte 2 dell'esercitazione, è possibile utilizzare questo metodo per visualizzare i corsi associati con il reparto che un utente seleziona dall'elenco a discesa.

Aprire *Courses.aspx* e passare a **progettazione** visualizzazione. Aggiungere un secondo `EntityDataSource` controllo alla pagina e denominarlo `CoursesEntityDataSource`. La connessione di `SchoolEntities` del modello e selezionare `Courses` come il **EntitySetName** valore.

Nel **proprietà** finestra, fare clic sui puntini di sospensione nel **dove** casella della proprietà. (Assicurarsi che il `CoursesEntityDataSource` controllo sia ancora selezionato prima di utilizzare il **proprietà** finestra.)

[![Image06](the-entity-framework-and-aspnet-getting-started-part-3/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image17.png)

Il **Editor espressioni** viene visualizzata la finestra di dialogo. In questa finestra di dialogo selezionare **genera automaticamente l'espressione in base ai parametri forniti**, quindi fare clic su **Aggiungi parametro**. Il parametro di nome `DepartmentID`selezionare **controllo** come il **origine parametro** valore e selezionare **DepartmentsDropDownList** come il **ControlID**  valore.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-3/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image19.png)

Fare clic su **Mostra proprietà avanzate**e il **proprietà** finestra del **Editor espressioni** nella finestra di dialogo Modifica il `Type` proprietà `Int32`.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-3/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image21.png)

Al termine, fare clic su **OK**.

Sotto l'elenco a discesa, aggiungere un `GridView` controllo alla pagina e denominarlo `CoursesGridView`. La connessione di `CoursesEntityDataSource` controllo origine dati, fare clic su **Aggiorna Schema**, fare clic su **Modifica colonne**e rimuovere il `DepartmentID` colonna. Il `GridView` markup del controllo è simile all'esempio seguente.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample1.aspx)]

Quando l'utente modifica il reparto selezionato nell'elenco a discesa, è opportuno l'elenco dei corsi associati per modificare automaticamente. Per rendere questo scopo, selezionare l'elenco a discesa e nel **proprietà** set finestra il `AutoPostBack` proprietà `True`.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-3/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image23.png)

Ora che si è terminato di utilizzare la finestra di progettazione, passare a **origine** consente di visualizzare e sostituire il `ConnectionString` e `DefaultContainer` nome proprietà del `CoursesEntityDataSource` controllare con il `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` attributo. Al termine, il markup per il controllo avrà un aspetto simile nell'esempio seguente.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample2.aspx)]

Eseguire la pagina e utilizzare l'elenco a discesa per selezionare diversi reparti. Solo i corsi offerti dal reparto selezionato vengono visualizzati nel `GridView` controllo.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-3/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image25.png)

## <a name="using-the-entitydatasource-groupby-property-to-group-data"></a>Utilizzando la proprietà di EntityDataSource "GroupBy" per raggruppare i dati

Si supponga che Contoso University desideri inserire alcune statistiche corpo studenti nella relativa pagina di informazioni. In particolare, che desidera visualizzare un riepilogo di un numero di studenti per la data che sono registrati.

Aprire *About*e in **origine** consente di visualizzare, sostituire il contenuto esistente del `BodyContent` controllare con "Student corpo statistiche" tra `h2` tag:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample3.aspx)]

Dopo l'intestazione, aggiungere un `EntityDataSource` controllare e denominarlo `StudentStatisticsEntityDataSource`. Connettersi a `SchoolEntities`, selezionare il `People` set di entità e lasciare il **selezionare** casella nella procedura guidata senza modificata. Impostare le proprietà seguenti **proprietà** finestra:

- Per filtrare solo per gli studenti, impostare il `Where` proprietà `it.EnrollmentDate is not null`.
- Per raggruppare i risultati in base alla data di registrazione, impostare il `GroupBy` proprietà `it.EnrollmentDate`.
- Per selezionare la data di registrazione e il numero di studenti, impostare il `Select` proprietà `it.EnrollmentDate, Count(it.EnrollmentDate) AS NumberOfStudents`.
- Per ordinare i risultati in base alla data di registrazione, impostare il `OrderBy` proprietà `it.EnrollmentDate`.

In **origine** consente di visualizzare, sostituire il `ConnectionString` e `DefaultContainer` nome di proprietà con un `ContextTypeName` proprietà. Il `EntityDataSource` markup del controllo è ora simile all'esempio seguente.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample4.aspx)]

La sintassi del `Select`, `GroupBy`, e `Where` proprietà simile a Transact-SQL, ad eccezione del `it` (parola chiave) che specifica l'entità corrente.

Aggiungere il markup seguente per creare un `GridView` controllo per visualizzare i dati.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample5.aspx)]

Eseguire la pagina per visualizzare un elenco che mostri il numero di studenti dalla data di registrazione.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image27.png)

## <a name="using-the-queryextender-control-for-filtering-and-ordering"></a>Utilizzando il controllo QueryExtender per il filtro e ordinamento

Il `QueryExtender` fornisce un modo per specificare il filtro e ordinamento nel markup. La sintassi è indipendente dal sistema di gestione di database (DBMS) in uso. È inoltre in genere indipendente di Entity Framework, con l'eccezione che la sintassi utilizzata per le proprietà di navigazione è univoca a Entity Framework.

In questa parte dell'esercitazione si userà un `QueryExtender` controllo per filtrare e ordinare i dati e uno dei campi order by è una proprietà di navigazione.

(Se si preferisce usare codice anziché markup per estendere le query che vengono generate automaticamente mediante il `EntityDataSource` controllo, è possibile farlo la gestione di `QueryCreated` evento. Questo è il modo in `QueryExtender` controllo si estende `EntityDataSource` controllare anche le query.)

Aprire il *Courses.aspx* pagina e sotto il codice aggiunto in precedenza, inserire il markup seguente per creare un'intestazione, una casella di testo per l'immissione di stringhe di ricerca, un pulsante di ricerca e un `EntityDataSource` controllo a cui è associato il `Courses` set di entità.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample6.aspx)]

Si noti che il `EntityDataSource` del controllo `Include` è impostata su `Department`. Nel database, il `Course` tabella non contiene il nome del reparto; contiene un `DepartmentID` colonna chiave esterna. Se si sono stati query direttamente, il database per ottenere il nome di reparto insieme ai dati del corso, è necessario creare un join di `Course` e `Department` tabelle. Impostando il `Include` proprietà `Department`, si specifica che Entity Framework deve eseguire il lavoro dell'attività correlata `Department` entità quando rileva un `Course` entità. Il `Department` entità vengono quindi archiviati nel `Department` proprietà di navigazione del `Course` entità. (Per impostazione predefinita, il `SchoolEntities` classe che è stato generato da Progettazione modelli di dati recupera i dati correlati quando necessario, e il controllo origine dati è stato associato a tale classe, pertanto l'impostazione di `Include` proprietà non è necessaria. Tuttavia, impostarlo migliora le prestazioni della pagina, poiché in caso contrario Entity Framework sarebbe più chiamate al database per recuperare dati per separare il `Course` entità e per l'oggetto correlato `Department` entità.)

Dopo il `EntityDataSource` controllo appena creato, inserire il markup seguente per creare un `QueryExtender` controllo associato a quella `EntityDataSource` controllo.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample7.aspx)]

Il `SearchExpression` elemento specifica che si desidera selezionare corsi il cui titolo corrisponde al valore immesso nella casella di testo. Solo come numero massimo di caratteri immessi nella casella di testo verrà confrontati perché il `SearchType` specifica proprietà `StartsWith`.

Il `OrderByExpression` elemento specifica che il set di risultati verrà ordinato titolo corso all'interno di nome del reparto. Si noti come specificare il nome del reparto: `Department.Name`. Poiché l'associazione tra il `Course` entità e `Department` entità è uno a uno, il `Department` contiene proprietà di navigazione un `Department` entità. (Se si trattasse di una relazione uno-a-molti, la proprietà può contenere una raccolta.) Per ottenere il nome di reparto, è necessario specificare il `Name` proprietà del `Department` entità.

Aggiungere infine un `GridView` controllo per visualizzare l'elenco dei corsi:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample8.aspx)]

La prima colonna è un campo di modello che visualizza il nome di reparto. Specifica l'espressione di associazione dati `Department.Name`, esattamente come specificato nella `QueryExtender` controllo.

Eseguire la pagina. La visualizzazione iniziale mostra un elenco di tutti i corsi in ordine dal reparto e quindi in base al titolo del corso.

[![image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image29.png)

Immettere una "m" e fare clic su **ricerca** per visualizzare tutti i corsi cui titoli iniziano con "m" (la ricerca è non distinzione maiuscole/minuscole).

[![Image12](the-entity-framework-and-aspnet-getting-started-part-3/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image31.png)

## <a name="using-the-like-operator-to-filter-data"></a>Utilizzo dell'operatore "Mi piace" per filtrare i dati

È possibile ottenere un effetto simile al `QueryExtender` del controllo `StartsWith`, `Contains`, e `EndsWith` Cerca tipi utilizzando un `Like` operatore nel `EntityDataSource` del controllo `Where` proprietà. In questa parte dell'esercitazione, si vedrà come utilizzare il `Like` operatore per cercare uno studente in base al nome.

Aprire *Students.aspx* in **origine** visualizzazione. Dopo il `GridView` di controllo, aggiungere il markup seguente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample9.aspx)]

Questo tag è simile a ciò che è stato illustrato in precedenza, ad eccezione del `Where` valore della proprietà. La seconda parte di `Where` espressione definisce la ricerca della sottostringa (`LIKE %FirstMidName% or LIKE %LastName%`) che nomi e cognomi per ciò che viene digitato nella casella di testo di ricerca.

Eseguire la pagina. Inizialmente verranno visualizzati tutti gli studenti perché il valore predefinito per il `StudentName` parametro è "%".

[![Image13](the-entity-framework-and-aspnet-getting-started-part-3/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image33.png)

Immettere la lettera "c" nella casella di testo e fare clic su **ricerca**. Viene visualizzato un elenco di studenti che hanno un "g" in entrambi il nome o cognome.

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image35.png)

Si è ora visualizzato, aggiornato, filtrati, ordinati e raggruppati i dati di singole tabelle. Nella prossima esercitazione verrà iniziare a lavorare con i dati correlati (scenari master-Details).

> [!div class="step-by-step"]
> [Precedente](the-entity-framework-and-aspnet-getting-started-part-2.md)
> [Successivo](the-entity-framework-and-aspnet-getting-started-part-4.md)
