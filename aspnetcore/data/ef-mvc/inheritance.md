---
title: ASP.NET Core MVC con EF Core - Ereditarietà - 9 di 10
author: rick-anderson
description: Questa esercitazione illustra come implementare l'ereditarietà nel modello di dati usando Entity Framework Core in un'applicazione ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-mvc/inheritance
ms.openlocfilehash: 60417040dd296311e1aecff8f224aadf8da82779
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090758"
---
# <a name="aspnet-core-mvc-with-ef-core---inheritance---9-of-10"></a>ASP.NET Core MVC con EF Core - Ereditarietà - 9 di 10

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

Di [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)

L'applicazione Web di esempio Contoso University illustra come creare applicazioni Web ASP.NET Core MVC con Entity Framework Core e Visual Studio. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](intro.md).

Nell'esercitazione precedente sono state presentate le eccezioni di concorrenza. In questa esercitazione viene illustrato come implementare l'ereditarietà nel modello di dati.

Nella programmazione orientata a oggetti è possibile usare l'ereditarietà per facilitare il riutilizzo del codice. In questa esercitazione verranno modificate le classi `Instructor` e `Student` in modo che derivino da una classe di base `Person` contenente proprietà quali `LastName` comuni a docenti e studenti. Non verranno aggiunte o modificate pagine Web, ma si modificherà parte del codice e le modifiche verranno automaticamente riflesse nel database.

## <a name="options-for-mapping-inheritance-to-database-tables"></a>Opzioni per il mapping dell'ereditarietà alle tabelle di database

Le classi `Instructor` e `Student` nel modello di dati School presentano molte proprietà identiche:

![Classi Student e Instructor](inheritance/_static/no-inheritance.png)

Si supponga di voler eliminare il codice ridondante per le proprietà condivise dalle entità `Instructor` e `Student`. Oppure che si desideri scrivere un servizio in grado di formattare i nomi senza sapere se il nome appartiene a un docente o a uno studente. È possibile creare una classe di base `Person` che contiene solo le proprietà condivise e quindi fare in modo che le classi `Instructor` e `Student` ereditino da questa classe di base, come illustrato nella figura seguente:

![Classi Student e Instructor derivanti dalla classe Person](inheritance/_static/inheritance.png)

Questa struttura di ereditarietà può essere rappresentata nel database in diversi modi. È possibile usare una tabella Person che includa le informazioni relative a studenti e docenti in un'unica tabella. Alcune colonne possono riguardare solo i docenti (HireDate), altre solo gli studenti (EnrollmentDate) e altre ancora entrambi (LastName, FirstName). Per indicare il tipo rappresentato da ogni riga viene in genere usata una colonna discriminante. La colonna discriminante può ad esempio indicare "Instructor" per i docenti e "Student" per gli studenti.

![Esempio di tabella per gerarchia](inheritance/_static/tph.png)

Questo criterio di generazione di una struttura di ereditarietà delle entità da una singola tabella di database è denominato ereditarietà tabella per gerarchia.

Un'alternativa consiste nel rendere il database più simile alla struttura di ereditarietà. È possibile, ad esempio, includere nella tabella Person solo i campi del nome e usare tabelle Instructor e Student separate con i campi della data.

![Ereditarietà tabella per tipo](inheritance/_static/tpt.png)

Questo criterio di creazione di una tabella di database per ogni classe di entità è denominato ereditarietà tabella per tipo.

Un'altra opzione consiste nell'eseguire il mapping di tutti i tipi non astratti a singole tabelle. Tutte le proprietà di una classe, incluse le proprietà ereditate, eseguono il mapping alle colonne della tabella corrispondente. Questo criterio è denominato ereditarietà della classe tabella per tipo concreto. Se per le classi Person, Student e Instructor è stata implementata l'ereditarietà tabella per tipo concreto come illustrato in precedenza, l'aspetto delle tabelle Student e Instructor dopo l'implementazione dell'ereditarietà non sarà diverso da quello precedente.

I criteri tabella per tipo concreto e tabella per gerarchia offrono in genere prestazioni migliori rispetto ai criteri tabella per tipo perché questi ultimi possono generare query join complesse.

Questa esercitazione illustra come implementare l'ereditarietà tabella per gerarchia. Il criterio di ereditarietà tabella per gerarchia è l'unico supportato da Entity Framework Core.  Verranno eseguite le operazioni di creazione di una classe `Person`, modifica delle classi `Instructor` e `Student` in modo che derivino da `Person`, aggiunta della nuova classe a `DbContext` e creazione di una migrazione.

> [!TIP]
> È consigliabile salvare una copia del progetto prima di apportare le modifiche seguenti.  In caso di problemi e se fosse necessario ricominciare da capo, sarà più facile iniziare dal progetto salvato invece di annullare i passaggi eseguiti per questa esercitazione o tornare all'inizio dell'intera serie.

## <a name="create-the-person-class"></a>Creare la classe Person

Nella cartella Models creare Person.cs e sostituire il codice del modello con il codice seguente:

[!code-csharp[](intro/samples/cu/Models/Person.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>Modificare le classi Student e Instructor in modo da stabilire l'ereditarietà da Person

In *Instructor.cs* derivare la classe Instructor dalla classe Person e rimuovere i campi chiave e nome. Il codice sarà simile all'esempio seguente:

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

Apportare le stesse modifiche in *Student.cs*.

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-the-person-entity-type-to-the-data-model"></a>Aggiungere il tipo di entità Person al modello di dati

Aggiungere il tipo di entità Person a *SchoolContext.cs*. Le nuove righe sono evidenziate.

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

L'ereditarietà tabella per gerarchia in Entity Framework è stata configurata. Come si vedrà, quando il database verrà aggiornato conterrà una tabella Person invece delle tabelle Student e Instructor.

## <a name="create-and-customize-migration-code"></a>Creare e personalizzare il codice di migrazione

Salvare le modifiche e compilare il progetto. Quindi aprire la finestra di comando nella cartella del progetto e immettere il comando seguente:

```console
dotnet ef migrations add Inheritance
```

Non eseguire ancora il comando `database update`. Questo comando determinerà la perdita di dati poiché eliminerà la tabella Instructor e rinominerà la tabella Student in Person. Per mantenere i dati esistenti è necessario specificare codice personalizzato.

Aprire *Migrations/\<timestamp>_Inheritance.cs* e sostituire il metodo `Up` con il codice seguente:

[!code-csharp[](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

Questo codice esegue le attività di aggiornamento del database seguenti:

* Rimuove i vincoli di chiave esterna e gli indici che puntano alla tabella Student.

* Rinomina la tabella Instructor in Person e apporta le modifiche necessarie perché sia in grado di archiviare i dati Student:

* Aggiunge un elemento EnrollmentDate che ammette i valori Null per gli studenti.

* Aggiunge una colonna discriminante che indica se una riga è per uno studente o un docente.

* Modifica HireDate in modo che ammetta i valori Null poiché le righe degli studenti non includono date di assunzione.

* Aggiunge un campo temporaneo che sarà usato per aggiornare le chiavi esterne che puntano agli studenti. Quando si copiano studenti nella tabella Person, questi otterranno nuovi valori di chiave primaria.

* Copia i dati dalla tabella Student alla tabella Person. Ciò comporta l'assegnazione di nuovi valori di chiave primaria agli studenti.

* Corregge i valori di chiave esterna che puntano agli studenti.

* Ricrea i vincoli di chiave esterna e gli indici che ora puntano alla tabella Person.

Se come tipo di chiave primaria fosse stato usato GUID anziché Integer, non sarebbe necessario modificare i valori di chiave primaria degli studenti e molti di questi passaggi avrebbero potuto essere omessi.

Eseguire il comando `database update`:

```console
dotnet ef database update
```

In un sistema di produzione verrebbero apportate modifiche corrispondenti al metodo `Down` nel caso fosse necessario usarlo per tornare alla versione precedente del database. Per questa esercitazione, il metodo `Down` non verrà usato.

> [!NOTE]
> Quando si apportano modifiche allo schema in un database con dati esistenti è possibile che si riscontrino altri errori. Se si verificano errori di migrazione che non si riesce a risolvere, è possibile modificare il nome del database nella stringa di connessione o eliminare il database. Un nuovo database non contiene dati di cui eseguire la migrazione e ci sono maggiori probabilità che il comando update-database venga completato senza errori. Per eliminare il database, usare SSOX o eseguire il comando dell’interfaccia della riga di comando `database drop`.

## <a name="test-with-inheritance-implemented"></a>Eseguire il test con l'ereditarietà implementata

Eseguire l'app e provare diverse pagine. Tutto funziona come in precedenza.

In **Esplora oggetti di SQL Server** espandere **Connessioni dati/SchoolContext** e quindi **Tabelle**. Si osserverà che le tabelle Student e Instructor sono state sostituite da una tabella Person. Dopo aver aperto la tabella Person in Progettazione tabelle si noterà che contiene tutte le colonne che in precedenza erano presenti nelle tabelle Student e Instructor.

![Tabella Person in SSOX](inheritance/_static/ssox-person-table.png)

Fare clic con il pulsante destro del mouse sulla tabella Person e quindi su **Mostra dati tabella** per vedere la colonna discriminante.

![Tabella Person in SSOX, dati della tabella](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a>Riepilogo

È stata implementata l'ereditarietà tabella per gerarchia per le classi `Person`, `Student` e `Instructor`. Per altre informazioni sull'ereditarietà in Entity Framework Core, vedere [Ereditarietà](/ef/core/modeling/inheritance). Nella prossima esercitazione si apprenderà come gestire diversi scenari Entity Framework relativamente avanzati.

::: moniker-end

> [!div class="step-by-step"]
> [Precedente](concurrency.md)
> [Successivo](advanced.md)
