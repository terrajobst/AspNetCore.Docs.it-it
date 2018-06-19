---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
title: Guida introduttiva a Database di Entity Framework 4.0 prima di tutto e ASP.NET 4 di Web Form - parte 6 | Documenti Microsoft
author: tdykstra
description: L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni Web Form ASP.NET tramite Entity Framework. È l'applicazione di esempio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 994a5496-c648-4830-b03c-55bb43f325d2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
msc.type: authoredcontent
ms.openlocfilehash: b76be25501275ba676c9a9acca8e73333439ee70
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888799"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-6"></a>Guida introduttiva a Database di Entity Framework 4.0 prima di tutto e form ASP.NET Web 4 - parte 6
====================
da [Tom Dykstra](https://github.com/tdykstra)

> L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni Web Form ASP.NET utilizzando il Entity Framework 4.0 e Visual Studio 2010. Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="implementing-table-per-hierarchy-inheritance"></a>Implementazione dell'ereditarietà tabella per gerarchia

Nell'esercitazione precedente si è lavorato con i dati correlati aggiunta ed eliminazione di relazioni e aggiungendo una nuova entità che ha una relazione a un'entità esistente. In questa esercitazione viene illustrato come implementare l'ereditarietà nel modello di dati.

Nella programmazione orientata agli oggetti, è possibile utilizzare l'ereditarietà per renderlo più facile lavorare con le classi correlate. Ad esempio, è possibile creare `Instructor` e `Student` classi che derivano da un `Person` classe di base. È possibile creare gli stessi tipi di strutture di ereditarietà tra le entità in Entity Framework.

In questa parte dell'esercitazione, non verranno create nuove pagine web. In alternativa, aggiungerai derivato entità nel modello di dati e modificare le pagine esistenti per utilizzare le nuove entità.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Tabella per gerarchia rispetto a ereditarietà tabella per tipo

Un database può archiviare informazioni sugli oggetti correlati in una tabella o in più tabelle. Ad esempio, nel `School` database, il `Person` tabella include informazioni su studenti e insegnanti in un'unica tabella. Alcune delle colonne si applicano solo per i docenti (`HireDate`), alcuni solo per gli studenti (`EnrollmentDate`) e alcuni a entrambi (`LastName`, `FirstName`).

[![image11](the-entity-framework-and-aspnet-getting-started-part-6/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image1.png)

È possibile configurare Entity Framework per creare `Instructor` e `Student` le entità che ereditano dal `Person` entità. Questo modello di generazione di una struttura di ereditarietà di entità da una singola tabella di database è denominato *della tabella per gerarchia* ereditarietà della.

Per i corsi, il `School` database utilizza un modello diverso. Corsi online e in loco corsi vengono archiviati in tabelle separate, ognuna delle quali dispone di una chiave esterna che punta al `Course` tabella. Informazioni comuni a entrambi i tipi di corso vengono archiviate solo nel `Course` tabella.

[![Image12](the-entity-framework-and-aspnet-getting-started-part-6/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image3.png)

È possibile configurare il modello di dati di Entity Framework in modo che `OnlineCourse` e `OnsiteCourse` entità ereditano il `Course` entità. Questo modello di generazione di una struttura di ereditarietà di entità da tabelle separate per ogni tipo, con ogni tabella separata di una tabella che archivia i dati comuni a tutti i tipi, viene denominato *tabella per tipo* ereditarietà (tabella per tipo).

Criteri di ereditarietà della tabella per gerarchia offrono in genere prestazioni migliori in Entity Framework di criteri di ereditarietà tabella per tipo, in quanto possono generare modelli di tabella per tipo query join complessi. Questa procedura dettagliata viene illustrato come implementare Implementerà l'ereditarietà. Che verranno eseguite eseguendo i passaggi seguenti:

- Creare `Instructor` e `Student` tipi di entità che derivano da `Person`.
- Sposta le proprietà relative alle entità derivata dal `Person` entità alle entità derivata.
- Impostare i vincoli su proprietà in tipi derivati.
- Rendere il `Person` entità un'entità astratta.
- Ogni mappa derivato entità per il `Person` tabella con una condizione che specifica la modalità determinare se un `Person` riga rappresenta che tipo derivato.

## <a name="adding-instructor-and-student-entities"></a>Aggiunta di entità Instructor e Student

Aprire la <em>SchoolModel</em> del file, fare doppio clic su un'area non occupata nella finestra di progettazione, seleziona <strong>Add</strong>, quindi selezionare <strong>entità</strong><em>.</em>

[![Image01](the-entity-framework-and-aspnet-getting-started-part-6/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image5.png)

Nel **Aggiungi entità** nome dell'entità nella finestra di dialogo `Instructor` e impostare il relativo **tipo di Base** opzione `Person`.

[![image02](the-entity-framework-and-aspnet-getting-started-part-6/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image7.png)

Fare clic su **OK**. La finestra di progettazione crea un `Instructor` entità da cui deriva il `Person` entità. La nuova entità non dispone ancora di qualsiasi proprietà.

[![image03](the-entity-framework-and-aspnet-getting-started-part-6/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image9.png)

Ripetere la procedura per creare un `Student` entità derivata da `Person`.

Solo i docenti avere date di assunzione, pertanto è necessario spostare la proprietà dal `Person` entità per il `Instructor` entità. Nel `Person` entità, fare doppio clic su di `HireDate` proprietà e fare clic su **Taglia**. Quindi fare doppio clic su **proprietà** nel `Instructor` entità e fare clic su **Incolla**.

[![image04](the-entity-framework-and-aspnet-getting-started-part-6/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image11.png)

La data di assunzione di un `Instructor` entità non può essere null. Fare doppio clic su di `HireDate` proprietà, fare clic su **proprietà**e quindi il **proprietà** Modifica finestra `Nullable` per `False`.

[![image05](the-entity-framework-and-aspnet-getting-started-part-6/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image13.png)

Ripetere la procedura per spostare il `EnrollmentDate` proprietà il `Person` entità per il `Student` entità. Assicurarsi inoltre impostare `Nullable` a `False` per il `EnrollmentDate` proprietà.

Ora che un `Person` entità include solo le proprietà comuni a `Instructor` e `Student` entità (ad eccezione delle proprietà di navigazione, che non si spostano), l'entità può essere utilizzato solo come un'entità di base nella struttura di ereditarietà. Pertanto, è necessario assicurare che non sia mai considerata come entità indipendenti. Fare doppio clic sul `Person` entità, selezionare **proprietà**e quindi il **proprietà** finestra modificare il valore della **astratta** proprietà  **True**.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-6/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image15.png)

## <a name="mapping-instructor-and-student-entities-to-the-person-table"></a>Mapping di entità Instructor e Student alla tabella Person

È ora necessario indicare a Entity Framework come distinguere `Instructor` e `Student` entità nel database.

Fare doppio clic su di `Instructor` entità e selezionare **tabella Mapping**. Nel **Dettagli Mapping** finestra, fare clic su **aggiungere una tabella o vista** e selezionare **persona**.

[![image07](the-entity-framework-and-aspnet-getting-started-part-6/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image17.png)

Fare clic su **aggiungere una condizione**, quindi selezionare **HireDate**.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-6/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image19.png)

Modifica **operatore** a **è** e **valore / proprietà** a **non Null**.

[![image10](the-entity-framework-and-aspnet-getting-started-part-6/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image21.png)

Ripetere la procedura per la `Students` entità, che specifica l'entità che esegue il mapping al `Person` tabella quando la `EnrollmentDate` colonna non è null. Successivamente, salvare e chiudere il modello di dati.

Compilare il progetto per creare nuove entità come classi e renderli disponibili nella finestra di progettazione.

## <a name="using-the-instructor-and-student-entities"></a>Utilizzando l'entità Instructor e Student

Al momento della creazione le pagine web che utilizzano dati student e instructor, è associato a dati per il `Person` è filtrata in base a e set di entità di `HireDate` o `EnrollmentDate` proprietà per limitare i dati restituiti a studenti o i docenti. Tuttavia, a questo punto quando si associa ogni controllo origine dati per il `Person` del set di entità, è possibile specificare che solo `Student` o `Instructor` tipi di entità devono essere selezionati. Poiché Entity Framework grado di distinguere gli studenti e insegnanti nel `Person` set di entità, è possibile rimuovere il `Where` le impostazioni di proprietà immesso manualmente a tale scopo.

Nella finestra di progettazione Visual Studio, è possibile specificare tipo di entità che un `EntityDataSource` controllo è necessario selezionare nel **EntityTypeFilter** casella di riepilogo a discesa del `Configure Data Source` procedura guidata, come illustrato nell'esempio seguente.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-6/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image23.png)

E il **proprietà** finestra è possibile rimuovere `Where` valori della clausola che non sono più necessari, come illustrato nell'esempio seguente.

[![image14](the-entity-framework-and-aspnet-getting-started-part-6/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image25.png)

Tuttavia, poiché è stato modificato il markup per `EntityDataSource` controlli da usare il `ContextTypeName` attributo, non è possibile eseguire il **Configura origine dati** procedura guidata in `EntityDataSource` i controlli che è già stato creato. Pertanto, si apporteranno le modifiche necessarie modificando invece markup.

Aprire il *Students.aspx* pagina. Nel `StudentsEntityDataSource` di controllo, rimuovere il `Where` attributo e aggiungere un `EntityTypeFilter="Student"` attributo. Il markup sarà ora simile all'esempio seguente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample1.aspx)]

L'impostazione di `EntityTypeFilter` attributo assicura che il `EntityDataSource` controllo selezionerà solo il tipo di entità specificato. Se si desidera recuperare gli `Student` e `Instructor` tipi di entità, non si imposta questo attributo. (È possibile scegliere di recupero di più tipi di entità con una `EntityDataSource` controllo solo se si utilizza il controllo di accesso ai dati di sola lettura. Se si utilizza un `EntityDataSource` di controllo per inserire, aggiornare o eliminare entità e se il set di entità è associato a può contenere più tipi, è possibile utilizzare solo con un tipo di entità e che è necessario impostare questo attributo.)

Ripetere la procedura per il `SearchEntityDataSource` controllare, ad eccezione del fatto solo la parte di rimuovere il `Where` attributo che consente di selezionare `Student` entità invece di rimuovere completamente la proprietà. Il tag di apertura del controllo sarà ora simile all'esempio seguente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample2.aspx)]

Eseguire la pagina per verificare che funzioni come in precedenza.

[![image15](the-entity-framework-and-aspnet-getting-started-part-6/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image27.png)

Aggiornare le pagine seguenti creati nelle esercitazioni precedenti in modo che utilizzino il nuovo `Student` e `Instructor` entità anziché `Person` entità, quindi eseguirli per verificare che funzionino come in precedenza:

- In *StudentsAdd.aspx*, aggiungere `EntityTypeFilter="Student"` per il `StudentsEntityDataSource` controllo. Il markup sarà ora simile all'esempio seguente: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample3.aspx)]

    [![image16](the-entity-framework-and-aspnet-getting-started-part-6/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image29.png)
- In *About*, aggiungere `EntityTypeFilter="Student"` per il `StudentStatisticsEntityDataSource` e rimuovere controllo `Where="it.EnrollmentDate is not null"`. Il markup sarà ora simile all'esempio seguente: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample4.aspx)]

    [![image17](the-entity-framework-and-aspnet-getting-started-part-6/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image31.png)
- In *Instructors.aspx* e *InstructorsCourses.aspx*, aggiungere `EntityTypeFilter="Instructor"` per il `InstructorsEntityDataSource` e rimuovere controllo `Where="it.HireDate is not null"`. Il markup in *Instructors.aspx* ora simile all'esempio seguente: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample5.aspx)]

    [![image18](the-entity-framework-and-aspnet-getting-started-part-6/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image33.png)

    Il markup in *InstructorsCourses.aspx* sarà ora simile all'esempio seguente:

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample6.aspx)]

    [![image19](the-entity-framework-and-aspnet-getting-started-part-6/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image35.png)

In seguito a queste modifiche, è stato migliorato manutenibilità dell'applicazione Contoso University in diversi modi. Sono state spostate della logica di selezione e convalida il livello di interfaccia utente (*aspx* markup) e renderlo parte integrante del livello di accesso ai dati. Questo aiuta a isolare il codice dell'applicazione di modifiche che è possibile apportare in futuro per lo schema del database o il modello di dati. Ad esempio, è possibile decidere che gli studenti potrebbero essere assunzione come ausilio docenti e pertanto ottengono una data di assunzione. È quindi possibile aggiungere una nuova proprietà per differenziare gli studenti da istruttori e aggiornare il modello di dati. Nessun codice nell'applicazione web dovrà essere modificato, ad eccezione di in cui si desidera visualizzare una data di assunzione per studenti. Un altro vantaggio dell'aggiunta di `Instructor` e `Student` entità è che il codice è più facilmente comprensibile, rispetto a quando definito `Person` gli oggetti che sono stati effettivamente studenti o i docenti.

Abbiamo visto come implementare un modello di ereditarietà in Entity Framework. Nell'esercitazione seguente, si apprenderà come usare stored procedure per avere maggiore controllo sulla modalità di Entity Framework accede al database.

> [!div class="step-by-step"]
> [Precedente](the-entity-framework-and-aspnet-getting-started-part-5.md)
> [Successivo](the-entity-framework-and-aspnet-getting-started-part-7.md)
