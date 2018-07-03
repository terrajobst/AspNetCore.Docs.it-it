---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Implementazione dell'ereditarietà con Entity Framework in un'applicazione ASP.NET MVC (8 di 10) | Microsoft Docs
author: tdykstra
description: L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: d3c3d19a62f601d007b0fac031ade1eef9f35771
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378936"
---
<a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a>Implementazione dell'ereditarietà con Entity Framework in un'applicazione ASP.NET MVC (8 di 10)
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto completato](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio 2012. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). È possibile avviare la serie di esercitazioni a partire dall'inizio oppure [scarica un progetto iniziale per questo capitolo](building-the-ef5-mvc4-chapter-downloads.md) e iniziare da qui.
> 
> > [!NOTE] 
> > 
> > Se si verifica un problema è possibile risolvere, [Scarica il capitolo completato](building-the-ef5-mvc4-chapter-downloads.md) e provare a riprodurre il problema. Confrontando il codice per il codice completo è generalmente possibile trovare la soluzione al problema. Per alcuni errori comuni e come risolverli, vedere [errori e soluzioni alternative.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Nell'esercitazione precedente si gestite le eccezioni di concorrenza. In questa esercitazione viene illustrato come implementare l'ereditarietà nel modello di dati.

Nella programmazione orientata agli oggetti, è possibile utilizzare l'ereditarietà per eliminare il codice ridondante. In questa esercitazione verranno modificate le classi `Instructor` e `Student` in modo che derivino da una classe di base `Person` contenente proprietà quali `LastName` comuni a docenti e studenti. Non verranno aggiunte o modificate pagine Web, ma si modificherà parte del codice e le modifiche verranno automaticamente riflesse nel database.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Tabella per gerarchia e dell'ereditarietà tabella per tipo

Nella programmazione orientata agli oggetti, è possibile utilizzare l'ereditarietà per renderlo più facile lavorare con le classi correlate. Ad esempio, il `Instructor` e `Student` le classi nel `School` modello di dati condividono diverse proprietà, che restituisce il codice ridondante:

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

Si supponga di voler eliminare il codice ridondante per le proprietà condivise dalle entità `Instructor` e `Student`. È possibile creare un `Person` classe base che contiene solo le proprietà condivise e quindi apportare le `Instructor` e `Student` entità ereditano da tale classe, come illustrato nella figura seguente:

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

Questa struttura di ereditarietà può essere rappresentata nel database in diversi modi. È possibile avere un `Person` tabella che include informazioni relative a studenti e docenti in un'unica tabella. Alcune colonne possono riguardare solo instructors (insegnanti) (`HireDate`), altre solo gli studenti (`EnrollmentDate`), alcuni a entrambi (`LastName`, `FirstName`). In genere, è necessario un *discriminatore* colonna per indicare quale tipo di ogni riga rappresenta. La colonna discriminante può ad esempio indicare "Instructor" per i docenti e "Student" per gli studenti.

![Tabella-per-hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

Il modello di generazione di una struttura di ereditarietà di entità da una singola tabella di database è definito *tabella per gerarchia* ereditarietà.

Un'alternativa consiste nel rendere il database più simile alla struttura di ereditarietà. Ad esempio, si potrebbero avere solo i campi del nome nel `Person` tabella e sono separati `Instructor` e `Student` tabelle con i campi di Data.

![Table-per-type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

Questo criterio di creazione di una tabella di database per ogni classe di entità è definita *tabella per tipo* ereditarietà (TPT).

Criteri di ereditarietà tabella per gerarchia offrono in genere prestazioni migliori in Entity Framework rispetto ai modelli di ereditarietà tabella per tipo, in quanto possono comportare modelli TPT query join complesse. Questa esercitazione illustra come implementare l'ereditarietà tabella per gerarchia. È possibile farlo attenendosi alla procedura seguente:

- Creare un `Person` classe e modificare i `Instructor` e `Student` classi di derivare da `Person`.
- Aggiungere codice di mapping del modello-database per la classe del contesto del database.
- Change `InstructorID` e `StudentID` riferimenti nel corso del progetto per `PersonID`.

## <a name="creating-the-person-class"></a>Creazione della classe Person

 Nota: Non sarà in grado di compilare il progetto dopo la creazione di classi sottostanti solo dopo aver aggiornato il controller che Usa queste classi. 

Nel *modelli* cartella, creare *Person.cs* e sostituire il codice del modello con il codice seguente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Nella *Instructor.cs*, derivare le `Instructor` classe il `Person` classe e rimuovere i campi chiavi e nome. Il codice sarà simile all'esempio seguente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Apportare modifiche analoghe a *Student.cs*. Il `Student` classe avrà un aspetto simile al seguente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a>Aggiunta di tipo di entità Person al modello

Nelle *SchoolContext.cs*, aggiungere un `DbSet` proprietà per il `Person` tipo di entità:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

L'ereditarietà tabella per gerarchia in Entity Framework è stata configurata. Come si vedrà, quando il database è stato nuovamente creato, avrà un `Person` tabella anziché il `Student` e `Instructor` tabelle.

## <a name="changing-instructorid-and-studentid-to-personid"></a>Modifica InstructorID e StudentID per PersonID

Nelle *SchoolContext.cs*, nell'istruzione mapping insegnante-corsi, cambiare `MapRightKey("InstructorID")` a `MapRightKey("PersonID")`:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

Questa modifica non è obbligatoria; Cambia semplicemente il nome della colonna InstructorID nella tabella di join molti-a-molti. Se si lascia il nome come InstructorID, l'applicazione potrebbe comunque funzionare correttamente. Di seguito è riportato il completate *SchoolContext.cs*:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

Successivamente è necessario modificare `InstructorID` al `PersonID` e `StudentID` a `PersonID` nel corso del progetto ***tranne*** nei file di timestamp migrazioni nel *migrazioni* cartella. A tale scopo è verrà trovare e aprire solo i file che devono essere modificati, quindi eseguire una modifica globale sui file aperti. L'unico file nei *migrazioni* è necessario modificare la cartella è *Migrations\Configuration.cs.*

1. > [!IMPORTANT]
   > Per iniziare è necessario chiudere tutti i file aperti in Visual Studio.
2. Fare clic su **trovare e sostituire: trovare tutti i file** nel **modificare** dal menu e quindi eseguire una ricerca per tutti i file nel progetto che contengono `InstructorID`.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. Aprire ogni file nei **risultati ricerca** finestra ***tranne*** il &lt;timestamp&gt;*\_. cs* i file di migrazione nel *Migrazioni* cartella, facendo doppio clic su una riga per ogni file.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. Aprire il **Sostituisci nei file** finestra di dialogo e modificare **Cerca in** al **tutti i documenti aperti**.
5. Usare la **Sostituisci nei file** finestra di dialogo per cambiare tutti `InstructorID` a `PersonID.`  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. Trovare tutti i file nel progetto che contengono `StudentID`.
7. Aprire ogni file nel **risultati ricerca** finestra ***tranne*** il &lt;timestamp&gt;*\_\*cs* i file di migrazione nel *migrazioni* cartella, facendo doppio clic su una riga per ogni file.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. Aprire il **Sostituisci nei file** finestra di dialogo e modificare **Cerca in** al **tutti i documenti aperti**.
9. Usare la **Sostituisci nei file** finestra di dialogo per modificare tutte `StudentID` a `PersonID`.   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. Compilare il progetto.

(Si noti che questo esempio dimostra una *svantaggio* del `classnameID` modello per la denominazione di chiavi primarie. Se fosse stata denominata senza aggiungervi come prefisso il nome della classe, ID di chiavi primarie *alcun* ridenominazione sarebbe necessaria adesso.)

## <a name="create-and-update-a-migrations-file"></a>Creare e aggiornare un File le migrazioni

In Package Manager Console (PMC), immettere il comando seguente:

`Add-Migration Inheritance`

Eseguire il `Update-Database` comando nella console di gestione pacchetti. Il comando avrà esito negativo a questo punto perché abbiamo i dati esistenti che le migrazioni non sa come gestire. Viene visualizzato l'errore seguente:

*L'istruzione ALTER TABLE è in conflitto con il vincolo FOREIGN KEY "FK\_dbo. Reparto\_dbo. Persona\_PersonID ". Il conflitto si è verificato nel database "ContosoUniversity", la tabella "dbo. Person", colonna 'PersonID'.*

Aprire *migrazioni\&lt; timestamp&gt;\_Inheritance.cs* e sostituire il `Up` metodo con il codice seguente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

Eseguire il `update-database` nuovo il comando.

> [!NOTE]
> È possibile ottenere altri errori durante la migrazione dei dati e apportare le modifiche dello schema. Se si verificano errori di migrazione non è possibile risolvere, è possibile continuare con l'esercitazione modificando la stringa di connessione nel *Web. config* file o l'eliminazione del database. L'approccio più semplice consiste nel rinominare il database di *Web. config* file. Ad esempio, modificare il nome del database in unità di capacità\_testare, come illustrato nell'esempio seguente:
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> Con un nuovo database, non sono presenti dati per eseguire la migrazione e il `update-database` comando è molto probabile che venga completato senza errori. Per istruzioni su come eliminare il database, vedere [come eliminare un Database da Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Se si adotta questo approccio per poter continuare con l'esercitazione, ignorare il passaggio di distribuzione alla fine di questa esercitazione, poiché il sito distribuito otterrebbe lo stesso errore quando viene eseguita automaticamente le migrazioni. Se si desidera risolvere un errore di migrazioni, la risorsa migliore è uno dei forum di Entity Framework o StackOverflow.com.


## <a name="testing"></a>Test

Esecuzione del sito e provare diverse pagine. Tutto funziona come in precedenza.

Nella **Esplora Server** espandere **SchoolContext** e quindi **tabelle**, e si può osservare che il **studente** e **Instructor**  le tabelle sono state sostituite da una **persona** tabella. Espandere la **Person** tabella e verificare che dispone di tutte le colonne che erano nel **studente** e **insegnante** tabelle.

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Fare clic con il pulsante destro del mouse sulla tabella Person e quindi su **Mostra dati tabella** per vedere la colonna discriminante.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Il diagramma seguente illustra la struttura del nuovo database School:

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a>Riepilogo

Ereditarietà tabella per gerarchia a questo punto è stata implementata per il `Person`, `Student`, e `Instructor` classi. Per altre informazioni su questa e altre strutture di ereditarietà, vedere [strategie di Mapping di ereditarietà](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) sul blog di Morteza Manavi. Nella prossima esercitazione si noterà alcuni modi per implementare il repository di modelli e unità di lavoro.

Sono disponibili collegamenti ad altre risorse di Entity Framework nel [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Precedente](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Successivo](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
