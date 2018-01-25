---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Implementazione dell'ereditarietà con Entity Framework 6 in un'applicazione ASP.NET MVC 5 (11 12) | Documenti Microsoft"
author: tdykstra
description: L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 5 con Code First di Entity Framework 6 e Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 118233338112a71216b909b1dabed2333bfa235e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="implementing-inheritance-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-11-of-12"></a>Implementazione dell'ereditarietà con Entity Framework 6 in un'applicazione ASP.NET MVC 5 (11 12)
====================
Da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) o [Scarica il PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 5 con Visual Studio 2013 e Code First di Entity Framework 6. Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Nell'esercitazione precedente si gestite le eccezioni di concorrenza. In questa esercitazione viene illustrato come implementare l'ereditarietà nel modello di dati.

Nella programmazione orientata a oggetti, è possibile utilizzare [ereditarietà](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) per facilitare [riutilizzo del codice](http://en.wikipedia.org/wiki/Code_reuse). In questa esercitazione si modificherà il `Instructor` e `Student` classi in modo che derivano da un `Person` classe che contiene, ad esempio proprietà di base `LastName` che sono comuni a entrambi i docenti e studenti. Non aggiungere o cambiare tutte le pagine web, ma si modificherà parte del codice e le modifiche verranno riflesse automaticamente nel database.

## <a name="options-for-mapping-inheritance-to-database-tables"></a>Opzioni per il mapping di ereditarietà alle tabelle di database

Il `Instructor` e `Student` classi di `School` modello di dati dispone di diverse proprietà che sono identiche:

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

Si supponga che si desidera eliminare il codice ridondante per le proprietà condivise dal `Instructor` e `Student` entità. O si desidera scrivere un servizio che è possibile formattare i nomi senza caring se il nome proviene da un docente o uno studente. È possibile creare un `Person` classe che contiene solo quelli condivisi di proprietà di base, quindi rendere la `Instructor` e `Student` che ereditano da tale classe di base, come illustrato nella figura seguente:

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Esistono diversi modi di che questa struttura di ereditarietà potrebbe essere rappresentata nel database. È possibile disporre di un `Person` tabella che include informazioni su studenti e insegnanti in un'unica tabella. Alcune delle colonne è stato possibile applicare solo per i docenti (`HireDate`), alcuni solo per gli studenti (`EnrollmentDate`), in alcune entrambe (`LastName`, `FirstName`). In genere, è necessario un *discriminatore* colonna per indicare quale tipo di ogni riga rappresenta. La colonna del discriminatore, ad esempio, potrebbe essere "Istruttore" per i docenti e "Student" per studenti.

![Table-per-hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Questo modello di generazione di una struttura di ereditarietà di entità da una singola tabella di database è denominato *della tabella per gerarchia* ereditarietà della.

In alternativa è possibile rendere il database più simile la struttura di ereditarietà. Ad esempio, si potrebbe avere solo i campi nome `Person` tabella e sono separate `Instructor` e `Student` le tabelle con i campi delle date.

![Table-per-type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Questo modello di esecuzione di una tabella di database per ogni classe di entità viene denominata *tabella per tipo* ereditarietà (tabella per tipo).

Ancora un'altra opzione consiste nell'eseguire il mapping di tutti i tipi non astratta per le singole tabelle. Tutte le proprietà di una classe, incluse le proprietà ereditate, eseguire il mapping alle colonne della tabella corrispondente. Questo modello viene denominato ereditarietà della classe per ogni tabella-Concrete (TPC). Se è implementato ereditarietà TPC per il `Person`, `Student`, e `Instructor` classi come illustrato in precedenza, il `Student` e `Instructor` tabelle risulterebbe non è diverse dopo l'implementazione dell'ereditarietà rispetto a quello precedente.

TPC e criteri di ereditarietà della tabella per gerarchia offrono in genere prestazioni migliori in Entity Framework di criteri di ereditarietà tabella per tipo, in quanto possono generare modelli di tabella per tipo query join complessi.

In questa esercitazione viene illustrato come implementare Implementerà l'ereditarietà. Della tabella per gerarchia è il modello di ereditarietà predefinite in Entity Framework, pertanto sufficiente creare un `Person` classe, modificare il `Instructor` e `Student` classi da cui derivare `Person`, aggiungere la nuova classe per il `DbContext`e creare un migrazione. (Per informazioni su come implementare altri modelli di ereditarietà, vedere [Mapping dell'ereditarietà per ogni tipo di tabella (tabella per tipo)](https://msdn.microsoft.com/data/jj591617#2.5) e [Mapping dell'ereditarietà di classe per ogni tabella-Concrete (TPC)](https://msdn.microsoft.com/data/jj591617#2.6) in MSDN Documentazione del Framework entità.)

## <a name="create-the-person-class"></a>Creare la classe di persona

Nel *modelli* cartella, creare *Person* e sostituire il codice del modello con il codice seguente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>Creazione delle classi Student e Instructor ereditare da una persona

In *Instructor.cs*, derivare la `Instructor` classe il `Person` classe e rimuovere i campi chiavi e il nome. Il codice sarà analogo al seguente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Apportare modifiche analoghe a *Student.cs*. La `Student` classe sarà analogo al seguente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-the-person-entity-type-to-the-model"></a>Aggiungere il tipo di entità utente al modello

In *SchoolContext.cs*, aggiungere un `DbSet` proprietà per il `Person` tipo di entità:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Questo è tutto ciò che necessita di Entity Framework per configurare l'ereditarietà tabella per gerarchia. Come si vedrà, quando il database viene aggiornato, si otterrà un `Person` tabella anziché il `Student` e `Instructor` tabelle.

## <a name="create-and-update-a-migrations-file"></a>Creare e aggiornare un File di migrazione

In Package Manager Console (PMC), immettere il comando seguente:

`Add-Migration Inheritance`

Eseguire il `Update-Database` comando PMC. Il comando avrà esito negativo a questo punto perché abbiamo migrazioni non sa come gestire i dati esistenti. Viene visualizzato un messaggio di errore simile a quello seguente:

> *Impossibile eliminare l'oggetto ' dbo. Istruttore ' perché vi fa riferimento un vincolo FOREIGN KEY.*


Aprire *migrazioni\&lt; timestamp&gt;\_Inheritance.cs* e sostituire il `Up` (metodo) con il codice seguente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

Questo codice si occupa delle attività di aggiornamento di database seguenti:

- Rimuove i vincoli foreign key e gli indici che fanno riferimento alla tabella di studenti.
- Rinomina la tabella Instructor come persona e apporta modifiche necessarie archiviare i dati di Student:

    - Aggiunge EnrollmentDate ammette valori null per studenti.
    - Aggiunge una colonna del discriminatore per indicare se una riga è per uno studente o un docente.
    - Poiché le righe di student avrà date di assunzione, rende HireDate ammette valori null.
    - Aggiunge un campo temporaneo che verrà utilizzato per aggiornare le chiavi esterne che puntano a studenti. Quando si copia studenti nella tabella Person sono otterrà nuovi valori di chiave primari.
- Copia i dati della tabella di studenti nella tabella Person. In questo modo gli studenti a vengano assegnati nuovi valori di chiave primari.
- Consente di correggere i valori di chiave esterna che fanno riferimento a studenti.
- Consente di ricreare i vincoli di chiave esterni e gli indici, ora puntarli alla tabella Person.

(Se è stato usato GUID anziché l'intero come tipo di chiave primario, non sarebbe necessario modificare i valori di chiave primaria di studenti e alcuni di questi passaggi Impossibile sono stati omessi.)

Eseguire il `update-database` nuovo il comando.

(In un sistema di produzione si renderebbe le modifiche corrispondenti al metodo verso il basso nel caso in cui qualora fosse necessario usarla per tornare alla versione del database precedente. Per questa esercitazione è non stia utilizzando il metodo verso il basso.)

> [!NOTE]
> È possibile ottenere altri errori durante la migrazione di dati e apportare le modifiche dello schema. Se si verificano errori di migrazione non è possibile risolvere, è possibile continuare con l'esercitazione modificando la stringa di connessione nel *Web. config* file o per l'eliminazione del database. L'approccio più semplice consiste nel rinominare il database di *Web. config* file. Ad esempio, modificare il nome del database per ContosoUniversity2, come illustrato nell'esempio seguente:
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
> 
> Con un nuovo database, non sono presenti dati per eseguire la migrazione e `update-database` comando è molto più probabile completata senza errori. Per istruzioni su come eliminare il database, vedere [come eliminare un Database da Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Se si adotta questo approccio per continuare l'esercitazione, ignorare il passaggio di distribuzione alla fine di questa esercitazione o la distribuzione in un nuovo sito e il database. Se si distribuisce un aggiornamento per il sito stesso che si è stati distribuzione già, EF otterrà lo stesso errore durante l'esecuzione di migrazioni automaticamente. Se si desidera risolvere un errore di migrazioni, la risorsa migliore è uno dei forum di Entity Framework o di StackOverflow.com.


## <a name="testing"></a>Test

Aprire il sito e provare diverse pagine. Tutto ciò che funziona come in precedenza.

In **Esplora Server,** espandere **dati Connections\SchoolContext** e quindi **tabelle**, noterai che il **Student** e **Istruttore** tabelle sono state sostituite da un **persona** tabella. Espandere il **persona** tabella e verificare che dispone di tutte le colonne utilizzate da tutti i **studente** e **istruttore** tabelle.

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Il pulsante destro della tabella Person e quindi fare clic su **Mostra dati tabella** per visualizzare la colonna del discriminatore.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Nel diagramma seguente viene illustrata la struttura del nuovo database School:

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a>Distribuire in Azure

In questa sezione è necessario aver completato l'opzione facoltativa **distribuzione dell'applicazione in Azure** sezione [parte 3, ordinamento, filtro e Paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) di questa serie di esercitazioni. Se si sono verificati errori di migrazioni risolti eliminando il database nel progetto locale, ignorare questo passaggio. o creare un nuovo sito e il database e distribuire il nuovo ambiente.

1. In Visual Studio, fare clic sul progetto in **Esplora** e selezionare **pubblica** dal menu di scelta rapida.  
  
    ![Pubblicare nel menu di scelta rapida progetto](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)
2. Fare clic su **Pubblica**.  
  
    ![publish](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)  
  
 L'app Web verrà aperto nel browser predefinito.
3. Testare l'applicazione per verificare il funzionamento.

    La prima volta che si esegue una pagina che accede al database, Entity Framework viene eseguito tutte le migrazioni `Up` metodi necessari per portare il database aggiornato con il modello di dati corrente.

## <a name="summary"></a>Riepilogo

Tabella per gerarchia ereditarietà per aver implementato il `Person`, `Student`, e `Instructor` classi. Per ulteriori informazioni su questa e altre strutture di ereditarietà, vedere [il modello di ereditarietà tabella per tipo](https://msdn.microsoft.com/data/jj618293) e [il modello di ereditarietà della tabella per gerarchia](https://msdn.microsoft.com/data/jj618292) su MSDN. Nella prossima esercitazione verrà visualizzato come gestire un'ampia gamma di scenari di Entity Framework relativamente avanzati.

Collegamenti ad altre risorse di Entity Framework, vedere il [accesso ai dati ASP.NET - risorse](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Precedente](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Successivo](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
