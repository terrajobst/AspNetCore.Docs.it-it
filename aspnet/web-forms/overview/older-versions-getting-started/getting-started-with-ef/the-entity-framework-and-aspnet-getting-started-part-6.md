---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
title: Introduzione a Entity Framework 4.0 Database First e ASP.NET 4 Web Form - parte 6 | Microsoft Docs
author: tdykstra
description: L'applicazione web di esempio Contoso University illustra come creare applicazioni Web Form ASP.NET utilizzando Entity Framework. L'applicazione di esempio è...
ms.author: aspnetcontent
ms.date: 12/03/2010
ms.assetid: 994a5496-c648-4830-b03c-55bb43f325d2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
msc.type: authoredcontent
ms.openlocfilehash: 1f0a0f050f74b41d6e33a406dfcf7a760c81321b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817434"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-6"></a>Introduzione a Entity Framework 4.0 Database First e ASP.NET 4 Web Form - parte 6
====================
da [Tom Dykstra](https://github.com/tdykstra)

> L'applicazione web di esempio Contoso University illustra come creare applicazioni Web Form ASP.NET usando il Entity Framework 4.0 e Visual Studio 2010. Per informazioni sulla serie di esercitazioni, vedere [la prima esercitazione della serie](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="implementing-table-per-hierarchy-inheritance"></a>Implementazione dell'ereditarietà tabella per gerarchia

Nell'esercitazione precedente è lavorare con i dati correlati aggiungendo ed eliminando le relazioni e aggiungendo una nuova entità che ha una relazione a un'entità esistente. In questa esercitazione viene illustrato come implementare l'ereditarietà nel modello di dati.

Nella programmazione orientata agli oggetti, è possibile utilizzare l'ereditarietà per renderlo più facile lavorare con le classi correlate. Ad esempio, è possibile creare `Instructor` e `Student` classi che derivano da un `Person` classe di base. È possibile creare gli stessi tipi di strutture di ereditarietà tra le entità in Entity Framework.

In questa parte dell'esercitazione, è non creare nuove pagine web. Al contrario, verrà aggiunto entità derivate nel modello di dati e modificare pagine esistenti per usare le nuove entità.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Tabella per gerarchia e dell'ereditarietà tabella per tipo

Un database può archiviare informazioni sugli oggetti correlati in una tabella o in più tabelle. Ad esempio, nelle `School` database, il `Person` tabella include informazioni relative a studenti e docenti in un'unica tabella. Alcune colonne si applicano solo a instructors (insegnanti) (`HireDate`), altre solo gli studenti (`EnrollmentDate`) e alcuni a entrambi (`LastName`, `FirstName`).

[![image11](the-entity-framework-and-aspnet-getting-started-part-6/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image1.png)

È possibile configurare Entity Framework per creare `Instructor` e `Student` entità che ereditano dal `Person` entità. Il modello di generazione di una struttura di ereditarietà di entità da una singola tabella di database è definito *tabella per gerarchia* ereditarietà.

Per i corsi, il `School` database utilizza un modello diverso. Corsi online e i corsi in sede vengono archiviati in tabelle separate, ognuna delle quali ha una chiave esterna che punta al `Course` tabella. Informazioni comuni a entrambi i tipi di corso vengono archiviate solo nel `Course` tabella.

[![image12](the-entity-framework-and-aspnet-getting-started-part-6/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image3.png)

È possibile configurare il modello di dati Entity Framework in modo che `OnlineCourse` e `OnsiteCourse` entità di ereditano il `Course` entità. Questo modello di generazione di una struttura di ereditarietà di entità da tabelle separate per ogni tipo, con ogni tabella separata Riferendosi di nuovo a una tabella che archivia i dati comuni a tutti i tipi, viene chiamato *tabella per tipo* ereditarietà (TPT).

Criteri di ereditarietà tabella per gerarchia offrono in genere prestazioni migliori in Entity Framework rispetto ai modelli di ereditarietà tabella per tipo, in quanto possono comportare modelli TPT query join complesse. Questa procedura dettagliata viene illustrato come implementare l'ereditarietà. È possibile farlo attenendosi alla procedura seguente:

- Creare `Instructor` e `Student` tipi di entità che derivano da `Person`.
- Spostare le proprietà relative alle entità derivata dal `Person` entità alle entità derivata.
- Impostare vincoli sulle proprietà in tipi derivati.
- Rendere il `Person` entità un'entità astratta.
- Ogni mappa derivato entità per il `Person` tabella con una condizione che specifica la modalità determinare se un `Person` rappresenta di tipo derivato di riga.

## <a name="adding-instructor-and-student-entities"></a>Aggiunta di entità Instructor e Student

Aprire il <em>SchoolModel</em> del file, fare doppio clic su un'area non occupata nella finestra di progettazione, seleziona <strong>Add</strong>, quindi selezionare <strong>entità</strong><em>.</em>

[![Image01](the-entity-framework-and-aspnet-getting-started-part-6/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image5.png)

Nel **Aggiungi entità** della finestra di dialogo Nome entità `Instructor` e impostare relativo **tipo di Base** possibilità `Person`.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-6/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image7.png)

Fare clic su **OK**. La finestra di progettazione crea un' `Instructor` entità che deriva dal `Person` entità. La nuova entità non dispone ancora di tutte le proprietà.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-6/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image9.png)

Ripetere la procedura per creare un `Student` entità che deriva anche da `Person`.

Solo instructors (insegnanti) includono date di assunzione, pertanto è necessario spostare la proprietà dal `Person` entità al `Instructor` entità. Nel `Person` entità, fare doppio clic il `HireDate` proprietà e fare clic su **Taglia**. Quindi fare doppio clic su **delle proprietà** nel `Instructor` entità e fare clic su **Incolla**.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-6/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image11.png)

La data di assunzione di un `Instructor` entità non può essere null. Fare doppio clic sul `HireDate` proprietà, fare clic su **proprietà**, quindi nel **proprietà** modificano `Nullable` a `False`.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-6/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image13.png)

Ripetere la procedura per spostare il `EnrollmentDate` proprietà dal `Person` entità al `Student` entità. Assicurarsi di impostare anche `Nullable` al `False` per il `EnrollmentDate` proprietà.

Ora che un `Person` entità include solo le proprietà comuni ai `Instructor` e `Student` entità (a parte le proprietà di navigazione, che non vengono spostati), l'entità può essere utilizzato solo come un'entità di base della struttura di ereditarietà. Pertanto, è necessario assicurare che non sia mai considerata come entità indipendenti. Fare doppio clic sul `Person` entità, selezionare **proprietà**e quindi nel **proprietà** finestra modificare il valore della **astratta** proprietà  **True**.

[![image06](the-entity-framework-and-aspnet-getting-started-part-6/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image15.png)

## <a name="mapping-instructor-and-student-entities-to-the-person-table"></a>Mapping di entità Instructor e Student alla tabella Person

È ora necessario indicare a Entity Framework come distinguere `Instructor` e `Student` entità del database.

Fare doppio clic il `Instructor` entità e selezionare **Mapping di tabelle**. Nel **Dettagli Mapping** finestra, fare clic su **aggiungere una tabella o vista** e selezionare **persona**.

[![image07](the-entity-framework-and-aspnet-getting-started-part-6/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image17.png)

Fare clic su **aggiungere una condizione**, quindi selezionare **HireDate**.

[![image09](the-entity-framework-and-aspnet-getting-started-part-6/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image19.png)

Change **operatore** al **viene** e **valore / proprietà** a **non Null**.

[![image10](the-entity-framework-and-aspnet-getting-started-part-6/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image21.png)

Ripetere la procedura per la `Students` entità, che specifica che l'entità è mappata al `Person` tabella quando il `EnrollmentDate` colonna non null. Quindi salvare e chiudere il modello di dati.

Compilare il progetto per creare le nuove entità come classi e renderli disponibili nella finestra di progettazione.

## <a name="using-the-instructor-and-student-entities"></a>Usando l'entità Instructor e Student

Durante la creazione di pagine web che usano i dati student e instructor, è associato a dati per il `Person` del set di entità e filtrati in base il `HireDate` o `EnrollmentDate` proprietà per limitare i dati restituiti per gli studenti o docenti. Tuttavia, a questo punto quando si associa ogni controllo origine dati per il `Person` del set di entità, è possibile specificare che solo `Student` o `Instructor` i tipi di entità devono essere selezionati. Perché Entity Framework in grado di differenziare gli studenti e docenti nel `Person` set di entità, è possibile rimuovere il `Where` le impostazioni di proprietà immesso manualmente a tale scopo.

Nella progettazione di Visual Studio, è possibile specificare tipo di entità che un `EntityDataSource` controllo dovrebbe selezionare nel **EntityTypeFilter** casella di riepilogo a discesa del `Configure Data Source` procedura guidata, come illustrato nell'esempio seguente.

[![image13](the-entity-framework-and-aspnet-getting-started-part-6/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image23.png)

E il **delle proprietà** finestra è possibile rimuovere `Where` valori clausola che non sono più necessari, come illustrato nell'esempio seguente.

[![Image14](the-entity-framework-and-aspnet-getting-started-part-6/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image25.png)

Tuttavia, poiché è stato modificato il markup per `EntityDataSource` controlli da usare il `ContextTypeName` attributo, non è possibile eseguire il **Configura origine dati** procedura guidata in `EntityDataSource` controlli che è già stato creato. Pertanto, si apporteranno le modifiche necessarie modificando invece markup.

Aprire il *Students.aspx* pagina. Nel `StudentsEntityDataSource` controllano, rimuovere il `Where` dell'attributo e aggiungere un `EntityTypeFilter="Student"` attributo. Il markup sarà ora simile all'esempio seguente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample1.aspx)]

Impostando il `EntityTypeFilter` attributo assicura che il `EntityDataSource` controllo selezionerà solo il tipo di entità specificato. Se si desidera recuperare entrambe `Student` e `Instructor` tipi di entità, non si imposta questo attributo. (È disponibile l'opzione di recupero di più tipi di entità con una `EntityDataSource` controllo solo se si usa il controllo per l'accesso ai dati di sola lettura. Se si usa un `EntityDataSource` controlla allo scopo di inserire, aggiornare o eliminare entità e se il set di entità è associato a può contenere più tipi, è possibile usare solo con un tipo di entità e che è necessario impostare questo attributo.)

Ripetere la procedura per la `SearchEntityDataSource` controllare, ad eccezione del fatto rimuovere solo la parte del `Where` attributo che consente di selezionare `Student` entità invece di rimuovere completamente la proprietà. Il tag di apertura del controllo sarà ora simile all'esempio seguente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample2.aspx)]

Eseguire la pagina per verificare che continuerà a funzionare come in precedenza.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-6/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image27.png)

Aggiornare le pagine seguenti creato nelle esercitazioni precedenti in modo che utilizzino il nuovo `Student` e `Instructor` entità anziché `Person` entità, quindi eseguirli per verificare che funzionino come in precedenza:

- Nelle *StudentsAdd.aspx*, aggiungere `EntityTypeFilter="Student"` per il `StudentsEntityDataSource` controllo. Il markup sarà ora simile all'esempio seguente: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample3.aspx)]

    [![image16](the-entity-framework-and-aspnet-getting-started-part-6/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image29.png)
- Nelle *About*, aggiungere `EntityTypeFilter="Student"` per il `StudentStatisticsEntityDataSource` controllare e rimuovere `Where="it.EnrollmentDate is not null"`. Il markup sarà ora simile all'esempio seguente: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample4.aspx)]

    [![image17](the-entity-framework-and-aspnet-getting-started-part-6/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image31.png)
- Nelle *Instructors.aspx* e *InstructorsCourses.aspx*, aggiungere `EntityTypeFilter="Instructor"` per i `InstructorsEntityDataSource` controllare e rimuovere `Where="it.HireDate is not null"`. Il markup *Instructors.aspx* ora simile al seguente: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample5.aspx)]

    [![image18](the-entity-framework-and-aspnet-getting-started-part-6/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image33.png)

    Il markup *InstructorsCourses.aspx* sarà ora simile all'esempio seguente:

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample6.aspx)]

    [![image19](the-entity-framework-and-aspnet-getting-started-part-6/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image35.png)

In seguito a queste modifiche, è stata migliorata la manutenibilità dell'applicazione Contoso University in diversi modi. Si è spostati logica di selezione e convalida dal livello dell'interfaccia utente (*aspx* markup) e ha reso parte integrante del livello di accesso ai dati. Ciò aiuta a isolare il codice dell'applicazione di modifiche che è possibile apportare in futuro per lo schema del database o il modello di dati. Ad esempio, è possibile decidere che gli studenti potrebbero assumere come ausilio degli insegnanti e pertanto otterrebbe una data di assunzione. È quindi possibile aggiungere una nuova proprietà per differenziare gli studenti da istruttori e aggiornare il modello di dati. Nessun codice nell'applicazione web dovrà essere modificato, ad eccezione di dove si desidera mostrare una data di assunzione per gli studenti. Un altro vantaggio dell'aggiunta `Instructor` e `Student` entità è che il codice sia più facilmente comprensibile rispetto a quando definito `Person` gli oggetti che erano in realtà studenti o instructors (insegnanti).

A questo punto si è appreso come implementare un modello di ereditarietà in Entity Framework. Nell'esercitazione seguente, si apprenderà come usare stored procedure per avere maggiore controllo sul modo in cui Entity Framework accede al database.

> [!div class="step-by-step"]
> [Precedente](the-entity-framework-and-aspnet-getting-started-part-5.md)
> [Successivo](the-entity-framework-and-aspnet-getting-started-part-7.md)
