---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
title: Introduzione a Entity Framework 4.0 Database First e ASP.NET 4 Web Form - parte 7 | Microsoft Docs
author: tdykstra
description: L'applicazione web di esempio Contoso University illustra come creare applicazioni Web Form ASP.NET utilizzando Entity Framework. L'applicazione di esempio è...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: f8afb245-b705-419c-8790-0b295e90d5e2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
msc.type: authoredcontent
ms.openlocfilehash: 48092838ac0b00137ae0a4e37391c31883afcd09
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836222"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-7"></a>Introduzione a Entity Framework 4.0 Database First e ASP.NET 4 Web Form - parte 7
====================
da [Tom Dykstra](https://github.com/tdykstra)

> L'applicazione web di esempio Contoso University illustra come creare applicazioni Web Form ASP.NET usando il Entity Framework 4.0 e Visual Studio 2010. Per informazioni sulla serie di esercitazioni, vedere [la prima esercitazione della serie](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-stored-procedures"></a>Utilizzo delle stored procedure

Nell'esercitazione precedente è implementato un modello di ereditarietà tabella per gerarchia. Questa esercitazione illustrerà come utilizzare le stored procedure per controllare ulteriormente l'accesso al database.

Entity Framework consente di specificare che è necessario utilizzare stored procedure per l'accesso al database. Per qualsiasi tipo di entità, è possibile specificare una stored procedure da utilizzare per la creazione, aggiornamento o l'eliminazione delle entità di quel tipo. Nel modello di dati è quindi possibile aggiungere riferimenti alle stored procedure che è possibile usare per eseguire attività quali il recupero di set di entità.

Utilizzo delle stored procedure è un requisito comune per l'accesso al database. In alcuni casi un amministratore del database può richiedere che l'accesso al database passano attraverso le stored procedure per motivi di sicurezza. In altri casi è possibile compilare una logica di business in alcuni dei processi di Entity Framework viene utilizzato quando si aggiorna il database. Ad esempio, ogni volta che viene eliminata un'entità è possibile copiarlo in un database di archiviazione. O ogni volta che viene aggiornata una riga si potrebbe voler scrivere una riga della tabella di registrazione che registra che ha apportato la modifica. È possibile eseguire questi tipi di attività in una stored procedure che viene chiamata ogni volta che Entity Framework elimina un'entità o aggiorna un'entità.

Come nell'esercitazione precedente, si creerà non tutte le nuove pagine. Al contrario, sarà necessario modificare il modo in cui che Entity Framework accede al database per alcune delle pagine è già stato creato.

In questa esercitazione si creerà le stored procedure nel database per l'inserimento `Student` e `Instructor` entità. È possibile aggiungerli al modello di dati e si specificano che Entity Framework devono usarli per l'aggiunta `Student` e `Instructor` entità al database. Si creerà inoltre una stored procedure che è possibile usare per recuperare `Course` entità.

## <a name="creating-stored-procedures-in-the-database"></a>Creazione di Stored procedure nel Database

(Se si usa la *School. mdf* file dal progetto disponibile per il download in questa esercitazione, è possibile ignorare questa sezione perché le stored procedure esistano già.)

Nelle **Esplora Server**, espandere *School. mdf*, fare doppio clic su **Stored Procedures**, selezionare **Aggiungi nuova Stored Procedure**.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-7/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image1.png)

Copiare le istruzioni SQL seguenti e incollarle nella finestra della stored procedure, sostituendo la stored procedure scheletro.

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample1.sql)]

[![Image14](the-entity-framework-and-aspnet-getting-started-part-7/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image3.png)

`Student` le entità hanno quattro proprietà: `PersonID`, `LastName`, `FirstName`, e `EnrollmentDate`. Il database genera automaticamente il valore ID e la stored procedure accetta parametri per le altre tre. La stored procedure restituisce il valore della chiave del record della nuova riga in modo che Entity Framework può tenere traccia di che nella versione dell'entità che conserva in memoria.

Salvare e chiudere la finestra di stored procedure.

Creare un `InsertInstructor` stored procedure allo stesso modo, usando le istruzioni SQL seguenti:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample2.sql)]

Creare `Update` stored procedure per il `Student` e `Instructor` entità anche. (Il database esiste già un `DeletePerson` stored procedure che funzionerà per entrambe `Instructor` e `Student` entities.)

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample3.sql)]

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample4.sql)]

In questa esercitazione verrà eseguito il mapping tutte le tre funzioni: insert, update e delete, per ogni tipo di entità. Consente a Entity Framework versione 4 è possibile mappare solo uno o due di queste funzioni alle stored procedure senza mapping di altri utenti, con una sola eccezione: se si esegue il mapping della funzione di aggiornamento, ma non della funzione di eliminazione, Entity Framework genererà un'eccezione quando si tentativo di eliminare un'entità. In Entity Framework versione 3.5, non è stato simile flessibilità nel mapping delle stored procedure: se è stata associata una funzione era necessario eseguire il mapping di tutte e tre.

Per creare una stored procedure che legge piuttosto che aggiorna i dati, crearne uno che seleziona tutti gli elementi `Course` entità, usando le istruzioni SQL seguenti:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample5.sql)]

## <a name="adding-the-stored-procedures-to-the-data-model"></a>Aggiunta delle Stored procedure al modello di dati

Le stored procedure vengono ora definite nel database, ma devono essere aggiunti al modello di dati per renderli disponibili per Entity Framework. Aprire *SchoolModel*, fare doppio clic nell'area di progettazione e selezionare **Aggiorna modello da Database**. Nel **Add** scheda della finestra di **Scegli oggetti di Database** finestra di dialogo, espandere **Stored procedure**, selezionare le stored procedure appena create e il `DeletePerson` stored procedure e quindi fare clic su **fine**.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-7/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image5.png)

## <a name="mapping-the-stored-procedures"></a>Mapping delle Stored procedure

In Progettazione modelli di dati, fare doppio clic il `Student` entità e selezionare **Mapping Stored Procedure**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-7/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image7.png)

Il **Dettagli Mapping** viene visualizzata la finestra in cui è possibile specificare le stored procedure che Entity Framework devono usare per l'inserimento, aggiornamento ed eliminazione di entità di questo tipo.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-7/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image9.png)

Impostare il **inserire** funzione **InsertStudent**. La finestra Mostra un elenco di parametri delle stored procedure, ognuno dei quali deve essere mappato a una proprietà di entità. Due di queste vengono mappate automaticamente perché i nomi sono uguali. È presente nessuna proprietà di entità denominata `FirstName`, pertanto è necessario selezionare manualmente `FirstMidName` da un elenco di riepilogo che mostra le proprietà delle entità disponibili. (Infatti, è stato modificato il nome del `FirstName` proprietà `FirstMidName` nella prima esercitazione.)

[![Image23](the-entity-framework-and-aspnet-getting-started-part-7/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image11.png)

Nella stessa **Dettagli Mapping** (finestra), mappa il `Update` funzione per il `UpdateStudent` stored procedure di (assicurarsi di specificare `FirstMidName` come valore del parametro per `FirstName`, come fatto in precedenza per il `Insert` stored procedure) e il `Delete` funzione per il `DeletePerson` stored procedure.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-7/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image13.png)

Seguire la stessa procedura per eseguire il mapping di insert, update e delete di stored procedure per instructors (insegnanti) per il `Instructor` entità.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-7/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image15.png)

Per le stored procedure che leggono invece di aggiornare i dati, si utilizza il **Browser modello** restituisce digitarlo finestra per eseguire il mapping della stored procedure per l'entità. In Progettazione modelli di dati, fare doppio clic su area di progettazione e seleziona **Browser modello**. Aprire il **SchoolModel. Store** nodo, quindi aprire il **Stored Procedures** nodo. Quindi scegliere il `GetCourses` stored procedure e selezionare **Aggiungi importazione di funzioni**.

[![image24](the-entity-framework-and-aspnet-getting-started-part-7/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image17.png)

Nel **Aggiungi importazione di funzioni** nella finestra di dialogo **restituisce una raccolta di** seleziona **entità**e quindi selezionare `Course` come il tipo di entità restituite. Al termine, fare clic su **OK**. Salvare e chiudere il *edmx* file.

[![image25](the-entity-framework-and-aspnet-getting-started-part-7/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image19.png)

## <a name="using-insert-update-and-delete-stored-procedures"></a>Utilizza inserimento, aggiornamento ed eliminazione di Stored procedure

Le stored procedure per inserire, aggiornare ed eliminare i dati vengono usati da Entity Framework automaticamente dopo aver aggiunte al modello di dati e li mappati alle entità appropriata. È ora possibile eseguire la *StudentsAdd.aspx* pagina, e ogni volta che si crea un nuovo studente, Entity Framework userà il `InsertStudent` stored procedure per aggiungere la nuova riga per il `Student` tabella.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-7/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image21.png)

Eseguire la *Students.aspx* pagina e il nuovo studente viene visualizzato nell'elenco.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-7/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image23.png)

Modificare il nome da verificare il corretto funzionamento della funzione di aggiornamento e quindi eliminare lo studente per verificare il corretto funzionamento della funzione di eliminazione.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-7/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image25.png)

## <a name="using-select-stored-procedures"></a>Utilizzo delle Stored procedure Select

Entity Framework non esegue automaticamente le stored procedure, ad esempio `GetCourses`, e non possono essere utilizzati con il `EntityDataSource` controllo. A questo scopo, si chiamarli dal codice.

Aprire il *InstructorsCourses.aspx.cs* file. Il `PopulateDropDownLists` metodo utilizza una query LINQ-a-entità per recuperare tutte le entità course in modo che possa scorrere l'elenco in ciclo e determinare quali un insegnante è assegnato a e quelle che sono assegnate:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample6.cs)]

Sostituire con il codice seguente:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample7.cs)]

La pagina Usa ora la `GetCourses` stored procedure per recuperare l'elenco di tutti i corsi. Eseguire la pagina per verificarne il funzionamento come in precedenza.

(Le proprietà di navigazione di entità recuperata da una stored procedure potrebbero non venire popolate automaticamente con i dati relativi a tali entità, a seconda `ObjectContext` impostazioni predefinite. Per altre informazioni, vedere [caricamento di oggetti correlati](https://msdn.microsoft.com/library/bb896272.aspx) in MSDN Library.)

Nella prossima esercitazione, si apprenderà come usare la funzionalità di Dynamic Data per renderlo più facile da programmare e testare i dati formattazione e convalida le regole. Anziché specificare le regole di ogni pagina web, ad esempio le stringhe di formato di dati e o meno un campo è obbligatorio, è possibile specificare regole di questo tipo nei metadati del modello di dati e vengono applicati automaticamente in ogni pagina.

> [!div class="step-by-step"]
> [Precedente](the-entity-framework-and-aspnet-getting-started-part-6.md)
> [Successivo](the-entity-framework-and-aspnet-getting-started-part-8.md)
