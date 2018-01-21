---
title: "Core di ASP.NET MVC con Entity Framework Core - ereditarietà - 9 di 10"
author: tdykstra
description: "In questa esercitazione viene illustrato come implementare l'ereditarietà nel modello di dati, l'utilizzo di Entity Framework Core in un'applicazione ASP.NET Core."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/inheritance
ms.openlocfilehash: 756f1bbba73bd760f780d18c01597642dd1f7216
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="inheritance---ef-core-with-aspnet-core-mvc-tutorial-9-of-10"></a>Ereditarietà - EF Core con l'esercitazione di base di ASP.NET MVC (9 di 10)

Da [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)

L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni web ASP.NET MVC di base con Entity Framework Core e Visual Studio. Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](intro.md).

Nell'esercitazione precedente, si gestite le eccezioni di concorrenza. In questa esercitazione viene illustrato come implementare l'ereditarietà nel modello di dati.

Nella programmazione orientata agli oggetti, è possibile utilizzare l'ereditarietà per facilitare il riutilizzo del codice. In questa esercitazione si modificherà il `Instructor` e `Student` classi in modo che derivano da un `Person` classe che contiene, ad esempio proprietà di base `LastName` che sono comuni a entrambi i docenti e studenti. Non aggiungere o cambiare tutte le pagine web, ma si modificherà parte del codice e le modifiche verranno riflesse automaticamente nel database.

## <a name="options-for-mapping-inheritance-to-database-tables"></a>Opzioni per il mapping di ereditarietà alle tabelle di database

Il `Instructor` e `Student` classi nel modello di dati dell'istituto di istruzione di sono associate diverse proprietà che sono identiche:

![Classi di Student e Instructor](inheritance/_static/no-inheritance.png)

Si supponga che si desidera eliminare il codice ridondante per le proprietà condivise dal `Instructor` e `Student` entità. O si desidera scrivere un servizio che è possibile formattare i nomi senza caring se il nome proviene da un docente o uno studente. È possibile creare un `Person` classe base che contiene solo le proprietà condivise, quindi rendere la `Instructor` e `Student` le classi ereditano da tale classe di base, come illustrato nella figura seguente:

![Student e Instructor le classi che derivano dalla classe di persona](inheritance/_static/inheritance.png)

Esistono diversi modi di che questa struttura di ereditarietà potrebbe essere rappresentata nel database. È possibile usare una tabella di persona che include informazioni su studenti e insegnanti in un'unica tabella. Alcune delle colonne è stato possibile applicare solo per i docenti (HireDate), alcuni solo per gli studenti (EnrollmentDate), alcune su entrambi (LastName, FirstName). In genere è una colonna del discriminatore per indicare che ogni riga rappresenta un tipo. La colonna del discriminatore, ad esempio, potrebbe essere "Istruttore" per i docenti e "Student" per studenti.

![Esempio di tabella per gerarchia](inheritance/_static/tph.png)

Questo modello di generazione di una struttura di ereditarietà di entità da una singola tabella di database è denominato tabella per gerarchia ereditarietà della.

In alternativa è possibile rendere il database più simile la struttura di ereditarietà. Ad esempio, è possibile avere solo i campi del nome della tabella Person e dispongono di tabelle di corsi e studenti separate con i campi delle date.

![Ereditarietà tabella per tipo](inheritance/_static/tpt.png)

Questo modello di una tabella di database per ogni classe di entità è denominato tabella per l'ereditarietà di tipo.

Ancora un'altra opzione consiste nell'eseguire il mapping di tutti i tipi non astratta per le singole tabelle. Tutte le proprietà di una classe, incluse le proprietà ereditate, eseguire il mapping alle colonne della tabella corrispondente. Questo modello viene denominato ereditarietà della classe per ogni tabella-Concrete (TPC). Se è implementato ereditarietà TPC per le classi di persona, Student e Instructor come illustrato in precedenza, le tabelle Student e Instructor avrà un aspetto non è diverse dopo l'implementazione dell'ereditarietà rispetto a quello precedente.

TPC e Implementerà i criteri di ereditarietà offrono in genere prestazioni migliori rispetto a criteri di ereditarietà tabella per tipo, in quanto possono generare modelli di tabella per tipo query join complessi.

In questa esercitazione viene illustrato come implementare Implementerà l'ereditarietà. Della tabella per gerarchia è il modello di ereditarietà solo che supporta il nucleo di Entity Framework.  È possibile eseguire è creare un `Person` classe, modificare il `Instructor` e `Student` classi da cui derivare `Person`, aggiungere la nuova classe per il `DbContext`e creare una migrazione.

> [!TIP] 
> Si consiglia di salvare una copia del progetto prima di apportare le modifiche seguenti.  Se si verificano problemi ed è necessario ricominciare, sarà più facile iniziare dal progetto salvato anziché inversione passaggi da eseguire per questa esercitazione o a partire dall'inizio della serie intera.

## <a name="create-the-person-class"></a>Creare la classe di persona

Nella cartella Models, creare Person e sostituire il codice del modello con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Models/Person.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>Creazione delle classi Student e Instructor ereditare da una persona

In *Instructor.cs*, derivare la classe istruttore dalla classe di persona e rimuovere i campi chiavi e il nome. Il codice sarà analogo al seguente:

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

Apportare le stesse modifiche nel *Student.cs*.

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-the-person-entity-type-to-the-data-model"></a>Aggiungere il tipo di entità utente per il modello di dati

Aggiungere il tipo di entità utente per *SchoolContext.cs*. Le nuove righe vengono evidenziate.

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

Questo è tutto ciò che necessita di Entity Framework per configurare l'ereditarietà tabella per gerarchia. Come si vedrà, quando il database viene aggiornato, si otterrà una tabella Person invece le tabelle Student e Instructor.

## <a name="create-and-customize-migration-code"></a>Creare e personalizzare il codice di migrazione

Salvare le modifiche e compilare il progetto. Quindi aprire la finestra di comando nella cartella del progetto e immettere il comando seguente:

```console
dotnet ef migrations add Inheritance
```

Non vengono eseguiti il `database update` ancora di comando. Tale comando causerà perdita di dati perché elimina la tabella Instructor e rinominare la tabella di studenti alla persona. È necessario fornire codice personalizzato per mantenere i dati esistenti.

Aprire *migrazioni /\<timestamp > _Inheritance.cs* e sostituire il `Up` (metodo) con il codice seguente:

[!code-csharp[Main](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

Questo codice si occupa delle attività di aggiornamento di database seguenti:

* Rimuove i vincoli foreign key e gli indici che fanno riferimento alla tabella di studenti.

* Rinomina la tabella Instructor come persona e apporta modifiche necessarie archiviare i dati di Student:

* Aggiunge EnrollmentDate ammette valori null per studenti.

* Aggiunge una colonna del discriminatore per indicare se una riga è per uno studente o un docente.

* Poiché le righe di student avrà date di assunzione, rende HireDate ammette valori null.

* Aggiunge un campo temporaneo che verrà utilizzato per aggiornare le chiavi esterne che puntano a studenti. Quando si copia studenti nella tabella Person sono otterrà nuovi valori di chiave primari.

* Copia i dati della tabella di studenti nella tabella Person. In questo modo gli studenti a vengano assegnati nuovi valori di chiave primari.

* Consente di correggere i valori di chiave esterna che fanno riferimento a studenti.

* Consente di ricreare i vincoli di chiave esterni e gli indici, ora puntarli alla tabella Person.

(Se è stato usato GUID anziché l'intero come tipo di chiave primario, non sarebbe necessario modificare i valori di chiave primaria di studenti e alcuni di questi passaggi Impossibile sono stati omessi.)

Eseguire il `database update` comando:

```console
dotnet ef database update
```

(In un sistema di produzione apportate le modifiche corrispondenti al `Down` metodo nel caso qualora fosse necessario usarla per tornare alla versione del database precedente. Per questa esercitazione non è possibile utilizzare il `Down` metodo.)

> [!NOTE] 
> È possibile ottenere altri errori quando si apportano modifiche allo schema in un database che contiene dati esistenti. Se si verificano errori di migrazione che è possibile risolvere, è possibile modificare il nome del database nella stringa di connessione o eliminare il database. Con un nuovo database, non sono presenti dati per eseguire la migrazione e il comando update-database è più probabile che completata senza errori. Per eliminare il database, utilizzare sillaba SSOX o eseguire il `database drop` comando CLI.

## <a name="test-with-inheritance-implemented"></a>Eseguire il test con ereditarietà implementato

Eseguire l'app e provare diverse pagine. Tutto ciò che funziona come in precedenza.

In **Esplora oggetti di SQL Server**, espandere **connessioni dati/SchoolContext** e quindi **tabelle**, noterai che le tabelle Student e Instructor sono state sostituite da un Tabella Person. Aprire Progettazione tabelle Person e si noterà che contiene tutte le colonne utilizzate per le tabelle Student e Instructor.

![Tabella Person in sillaba SSOX](inheritance/_static/ssox-person-table.png)

Il pulsante destro della tabella Person e quindi fare clic su **Mostra dati tabella** per visualizzare la colonna del discriminatore.

![Tabella Person in sillaba SSOX - i dati della tabella](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a>Riepilogo

Tabella per gerarchia ereditarietà per aver implementato il `Person`, `Student`, e `Instructor` classi. Per ulteriori informazioni sull'ereditarietà in Entity Framework Core, vedere [ereditarietà](https://docs.microsoft.com/ef/core/modeling/inheritance). Nella prossima esercitazione verrà visualizzato come gestire un'ampia gamma di scenari di Entity Framework relativamente avanzati.

>[!div class="step-by-step"]
[Precedente](concurrency.md)
[Successivo](advanced.md)  
