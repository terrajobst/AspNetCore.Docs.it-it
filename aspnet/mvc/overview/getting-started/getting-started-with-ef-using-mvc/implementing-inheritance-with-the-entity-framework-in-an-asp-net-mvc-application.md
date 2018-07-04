---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Implementazione dell'ereditarietà con Entity Framework 6 in un'applicazione ASP.NET MVC 5 (11 su 12) | Microsoft Docs
author: tdykstra
description: L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 5 con Entity Framework 6 Code First e Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: d10656f5ce0479e1290fa6921f2d1a99d1081b57
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382978"
---
<a name="implementing-inheritance-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-11-of-12"></a>Implementazione dell'ereditarietà con Entity Framework 6 in un'applicazione ASP.NET MVC 5 (11 su 12)
====================
da [Tom Dykstra](https://github.com/tdykstra)

[Download progetto completato](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) o [Scarica il PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> L'applicazione web di esempio Contoso University illustra come creare applicazioni ASP.NET MVC 5 con Entity Framework 6 Code First e Visual Studio 2013. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Nell'esercitazione precedente si gestite le eccezioni di concorrenza. In questa esercitazione viene illustrato come implementare l'ereditarietà nel modello di dati.

Nella programmazione orientata agli oggetti, è possibile usare [ereditarietà](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) per facilitare [riutilizzo del codice](http://en.wikipedia.org/wiki/Code_reuse). In questa esercitazione verranno modificate le classi `Instructor` e `Student` in modo che derivino da una classe di base `Person` contenente proprietà quali `LastName` comuni a docenti e studenti. Non verranno aggiunte o modificate pagine Web, ma si modificherà parte del codice e le modifiche verranno automaticamente riflesse nel database.

## <a name="options-for-mapping-inheritance-to-database-tables"></a>Opzioni per il mapping dell'ereditarietà alle tabelle di database

Il `Instructor` e `Student` le classi di `School` modello di dati sono associate diverse proprietà che sono identiche:

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

Si supponga di voler eliminare il codice ridondante per le proprietà condivise dalle entità `Instructor` e `Student`. Oppure che si desideri scrivere un servizio in grado di formattare i nomi senza sapere se il nome appartiene a un docente o a uno studente. È possibile creare un `Person` classe base che contiene solo le proprietà condivise e quindi apportare le `Instructor` e `Student` entità ereditano da tale classe, come illustrato nella figura seguente:

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Questa struttura di ereditarietà può essere rappresentata nel database in diversi modi. È possibile avere un `Person` tabella che include informazioni relative a studenti e docenti in un'unica tabella. Alcune colonne possono riguardare solo instructors (insegnanti) (`HireDate`), altre solo gli studenti (`EnrollmentDate`), alcuni a entrambi (`LastName`, `FirstName`). In genere, è necessario un *discriminatore* colonna per indicare quale tipo di ogni riga rappresenta. La colonna discriminante può ad esempio indicare "Instructor" per i docenti e "Student" per gli studenti.

![Tabella-per-hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Il modello di generazione di una struttura di ereditarietà di entità da una singola tabella di database è definito *tabella per gerarchia* ereditarietà.

Un'alternativa consiste nel rendere il database più simile alla struttura di ereditarietà. Ad esempio, si potrebbero avere solo i campi del nome nel `Person` tabella e sono separati `Instructor` e `Student` tabelle con i campi di Data.

![Table-per-type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Questo criterio di creazione di una tabella di database per ogni classe di entità è definita *tabella per tipo* ereditarietà (TPT).

Un'altra opzione consiste nell'eseguire il mapping di tutti i tipi non astratti a singole tabelle. Tutte le proprietà di una classe, incluse le proprietà ereditate, eseguono il mapping alle colonne della tabella corrispondente. Questo criterio è denominato ereditarietà della classe tabella per tipo concreto. Se è stata implementata l'ereditarietà TPC per il `Person`, `Student`, e `Instructor` classi come illustrato in precedenza, il `Student` e `Instructor` aspetto alcuna differenza delle tabelle dopo l'implementazione dell'ereditarietà rispetto a quello precedente.

Concreto e criteri di ereditarietà tabella per gerarchia offrono in genere prestazioni migliori in Entity Framework rispetto ai modelli di ereditarietà tabella per tipo, in quanto possono comportare modelli TPT query join complesse.

Questa esercitazione illustra come implementare l'ereditarietà tabella per gerarchia. Tabella per gerarchia è il modello di ereditarietà predefinita in Entity Framework, in modo che tutto è necessario eseguire è creare un `Person` classe, modificare il `Instructor` e `Student` classi di derivare da `Person`, aggiungere la nuova classe il `DbContext`e creare un migrazione. (Per informazioni su come implementare altri modelli di ereditarietà, vedere [Mapping dell'ereditarietà tabella Per tipo TPT ()](https://msdn.microsoft.com/data/jj591617#2.5) e [Mapping dell'ereditarietà tabella Per tipo concreto classe (TP)](https://msdn.microsoft.com/data/jj591617#2.6) in MSDN Documentazione di Entity Framework.)

## <a name="create-the-person-class"></a>Creare la classe Person

Nel *modelli* cartella, creare *Person.cs* e sostituire il codice del modello con il codice seguente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>Modificare le classi Student e Instructor in modo da stabilire l'ereditarietà da Person

Nella *Instructor.cs*, derivare le `Instructor` classe il `Person` classe e rimuovere i campi chiavi e nome. Il codice sarà simile all'esempio seguente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Apportare modifiche analoghe a *Student.cs*. Il `Student` classe avrà un aspetto simile al seguente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-the-person-entity-type-to-the-model"></a>Aggiungere il tipo di entità Person al modello

Nelle *SchoolContext.cs*, aggiungere un `DbSet` proprietà per il `Person` tipo di entità:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

L'ereditarietà tabella per gerarchia in Entity Framework è stata configurata. Come si vedrà, quando viene aggiornato il database, avrà un `Person` tabella anziché il `Student` e `Instructor` tabelle.

## <a name="create-and-update-a-migrations-file"></a>Creare e aggiornare un File le migrazioni

In Package Manager Console (PMC), immettere il comando seguente:

`Add-Migration Inheritance`

Eseguire il `Update-Database` comando nella console di gestione pacchetti. Il comando avrà esito negativo a questo punto perché abbiamo i dati esistenti che le migrazioni non sa come gestire. Viene visualizzato un messaggio di errore simile a quello seguente:

> *Impossibile eliminare l'oggetto ' dbo. Instructor' perché vi fanno riferimento tramite un vincolo FOREIGN KEY.*


Aprire *migrazioni\&lt; timestamp&gt;\_Inheritance.cs* e sostituire il `Up` metodo con il codice seguente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

Questo codice esegue le attività di aggiornamento del database seguenti:

- Rimuove i vincoli di chiave esterna e gli indici che puntano alla tabella Student.
- Rinomina la tabella Instructor in Person e apporta le modifiche necessarie perché sia in grado di archiviare i dati Student:

    - Aggiunge un elemento EnrollmentDate che ammette i valori Null per gli studenti.
    - Aggiunge una colonna discriminante che indica se una riga è per uno studente o un docente.
    - Modifica HireDate in modo che ammetta i valori Null poiché le righe degli studenti non includono date di assunzione.
    - Aggiunge un campo temporaneo che sarà usato per aggiornare le chiavi esterne che puntano agli studenti. Quando si copiano studenti nella tabella Person otterranno nuovi valori di chiave primari.
- Copia i dati dalla tabella Student alla tabella Person. Ciò comporta l'assegnazione di nuovi valori di chiave primaria agli studenti.
- Corregge i valori di chiave esterna che puntano agli studenti.
- Ricrea i vincoli di chiave esterna e gli indici che ora puntano alla tabella Person.

Se come tipo di chiave primaria fosse stato usato GUID anziché Integer, non sarebbe necessario modificare i valori di chiave primaria degli studenti e molti di questi passaggi avrebbero potuto essere omessi.

Eseguire il `update-database` nuovo il comando.

(In un sistema di produzione è verrebbero apportate modifiche corrispondenti al metodo verso il basso nel caso in cui fosse necessario usarlo per tornare alla versione precedente del database. Per questa esercitazione non utilizzeremo il metodo verso il basso.)

> [!NOTE]
> È possibile ottenere altri errori durante la migrazione dei dati e apportare le modifiche dello schema. Se si verificano errori di migrazione non è possibile risolvere, è possibile continuare con l'esercitazione modificando la stringa di connessione nel *Web. config* file o eliminando il database. L'approccio più semplice consiste nel rinominare il database di *Web. config* file. Ad esempio, modificare il nome del database in ContosoUniversity2 come illustrato nell'esempio seguente:
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
> 
> Con un nuovo database, non sono presenti dati per eseguire la migrazione e il `update-database` comando è molto probabile che venga completato senza errori. Per istruzioni su come eliminare il database, vedere [come eliminare un Database da Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Se si adotta questo approccio per poter continuare con l'esercitazione, ignorare il passaggio di distribuzione alla fine di questa esercitazione o la distribuzione in un nuovo sito e del database. Se si distribuisce un aggiornamento allo stesso sito di che cui è stato distribuito già, EF otterrà lo stesso errore quando viene eseguita automaticamente le migrazioni. Se si desidera risolvere un errore di migrazioni, la risorsa migliore è uno dei forum di Entity Framework o StackOverflow.com.


## <a name="testing"></a>Test

Esecuzione del sito e provare diverse pagine. Tutto funziona come in precedenza.

In **Esplora Server** espandere **dati Connections\SchoolContext** e quindi **tabelle**, e si può osservare che il **studente** e **Insegnante** le tabelle sono state sostituite da un **persona** tabella. Espandere la **Person** tabella e verificare che dispone di tutte le colonne che erano nel **studente** e **insegnante** tabelle.

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Fare clic con il pulsante destro del mouse sulla tabella Person e quindi su **Mostra dati tabella** per vedere la colonna discriminante.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Il diagramma seguente illustra la struttura del nuovo database School:

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a>Distribuire in Azure

In questa sezione è necessario aver completato l'opzione facoltativa **la distribuzione dell'app in Azure** sezione [parte 3, l'ordinamento, filtro e Paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) di questa serie di esercitazioni. Se si sono verificati errori di migrazione che è stato risolto, eliminare il database nel progetto locale, ignorare questo passaggio. o creare un nuovo sito e i database e distribuire nel nuovo ambiente.

1. In Visual Studio, fare clic sul progetto in **Esplora soluzioni** e selezionare **Publish** dal menu di scelta rapida.  
  
    ![Pubblicare nel menu di scelta rapida progetto](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)
2. Fare clic su **Pubblica**.  
  
    ![publish](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)  
  
   L'app Web verrà aperto nel browser predefinito.
3. Testare l'applicazione per verificare funzioni.

    Alla prima esecuzione di una pagina che accede al database, Entity Framework esegue tutte le migrazioni `Up` metodi necessari per portare il database aggiornato con il modello di dati corrente.

## <a name="summary"></a>Riepilogo

È stata implementata l'ereditarietà tabella per gerarchia per le classi `Person`, `Student` e `Instructor`. Per altre informazioni su questa e altre strutture di ereditarietà, vedere [modello di ereditarietà TPT](https://msdn.microsoft.com/data/jj618293) e [modello di ereditarietà tabella per gerarchia](https://msdn.microsoft.com/data/jj618292) su MSDN. Nella prossima esercitazione si apprenderà come gestire diversi scenari Entity Framework relativamente avanzati.

Sono disponibili collegamenti ad altre risorse di Entity Framework nel [l'accesso ai dati ASP.NET - risorse consigliate](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Precedente](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Successivo](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
