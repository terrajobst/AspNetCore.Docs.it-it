---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
title: Introduzione a Entity Framework 4.0 Database First e ASP.NET 4 Web Form - parte 5 | Microsoft Docs
author: tdykstra
description: L'applicazione web di esempio Contoso University illustra come creare applicazioni Web Form ASP.NET utilizzando Entity Framework. L'applicazione di esempio è...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: 24ad4379-3fb2-44dc-ba59-85fe0ffcb2ae
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
msc.type: authoredcontent
ms.openlocfilehash: 3209ab3bca58e0dde90cf279732d177418b034e4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828041"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-5"></a>Introduzione a Entity Framework 4.0 Database First e ASP.NET 4 Web Form - parte 5
====================
da [Tom Dykstra](https://github.com/tdykstra)

> L'applicazione web di esempio Contoso University illustra come creare applicazioni Web Form ASP.NET usando il Entity Framework 4.0 e Visual Studio 2010. Per informazioni sulla serie di esercitazioni, vedere [la prima esercitazione della serie](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data-continued"></a>Usa i dati correlati, continua

Nell'esercitazione precedente si è iniziato a usare il `EntityDataSource` per funzionare con i dati correlati. È visualizzato più livelli della gerarchia e modificare i dati nelle proprietà di navigazione. In questa esercitazione si continuerà a funzionare con i dati correlati aggiungendo ed eliminando le relazioni e aggiungendo una nuova entità che ha una relazione a un'entità esistente.

Si creerà una pagina che aggiunge i corsi assegnati a reparti. I reparti è già presente, e quando si crea un nuovo corso, allo stesso tempo è possibile stabilire una relazione tra i dati e un dipartimento esistente.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)

Si creerà inoltre una pagina che funziona con una relazione molti-a-molti assegnando un insegnante a un corso (aggiunta di una relazione tra due entità che si seleziona) o la rimozione di un docente di un corso (rimozione di una relazione tra due entità che si Selezionare). Nel database, aggiunta di una relazione tra un insegnante e un corso comporterà una nuova riga viene aggiunta al `CourseInstructor` tabella di associazione, la rimozione di una relazione comporta l'eliminazione di una riga dal `CourseInstructor` tabella di associazione. Tuttavia, eseguire questa operazione in Entity Framework impostando le proprietà di navigazione, senza fare riferimento al `CourseInstructor` tabella in modo esplicito.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)

## <a name="adding-an-entity-with-a-relationship-to-an-existing-entity"></a>Aggiunta di un'entità con una relazione a un'entità esistente

Creare una nuova pagina web denominata *CoursesAdd.aspx* che usa le *Site. master* pagina master e aggiungere il markup seguente per il `Content` controllo denominato `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample1.aspx)]

Questo codice crea un' `EntityDataSource` controllo che consente di selezionare i corsi, che consente l'inserimento e specifica un gestore per il `Inserting` evento. Si userà il gestore per aggiornare il `Department` proprietà di navigazione quando un nuovo `Course` entità viene creata.

Il markup crea anche un `DetailsView` controllo da utilizzare per l'aggiunta di un nuovo `Course` entità. Il markup Usa i campi associati per `Course` proprietà dell'entità. È necessario immettere il `CourseID` valore perché non è un campo ID generato dal sistema. In alternativa, è un numero di corso che deve essere specificato manualmente dopo aver creato il corso.

Si utilizza un modello di campo per il `Department` proprietà di navigazione perché le proprietà di navigazione non possono essere utilizzate con `BoundField` controlli. Campo modello presenta un elenco di riepilogo a discesa per selezionare il dipartimento. L'elenco a discesa associato ai `Departments` usando set di entità `Eval` anziché `Bind`, anche in questo caso poiché non è possibile associare direttamente le proprietà di navigazione per aggiornarli. Si specifica un gestore per il `DropDownList` del controllo `Init` eventi in modo che è possibile archiviare un riferimento al controllo per l'uso dal codice che aggiorna il `DepartmentID` chiave esterna.

Nelle *CoursesAdd.aspx.cs* subito dopo la dichiarazione di classe parziale, aggiungere un riferimento al campo di una classe di `DepartmentsDropDownList` controllo:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample2.cs)]

Aggiungere un gestore per il `DepartmentsDropDownList` del controllo `Init` eventi in modo che è possibile archiviare un riferimento al controllo. Ciò consente di ottenere il valore di cui l'utente ha immesso e utilizzarla per aggiornare il `DepartmentID` pari al `Course` entità.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample3.cs)]

Aggiungere un gestore per il `DetailsView` del controllo `Inserting` evento:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample4.cs)]

Quando l'utente sceglie `Insert`, il `Inserting` evento viene generato prima che venga inserito il nuovo record. Ottiene il codice nel gestore il `DepartmentID` dal `DropDownList` controllano e lo usa per impostare il valore che verrà usato per il `DepartmentID` proprietà del `Course` entità.

Entity Framework si occupa dell'aggiunta di questo corso per il `Courses` proprietà di navigazione dell'oggetto associato `Department` entità. Aggiunge anche il reparto per i `Department` proprietà di navigazione del `Course` entità.

Eseguire la pagina.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)

Immettere un ID, un titolo, un numero di crediti e selezionare un reparto, quindi fare clic su **Inserisci**.

Eseguire la *Courses.aspx* pagina e selezionare dello stesso reparto per visualizzare il nuovo corso.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)

## <a name="working-with-many-to-many-relationships"></a>Utilizzo delle relazioni molti-a-molti

La relazione tra il `Courses` set di entità e `People` set di entità è una relazione molti-a-molti. Oggetto `Course` entità dispone di una proprietà di navigazione denominata `People` che può contenere nessuno, uno o più correlate `Person` entità (che rappresentano instructors (insegnanti) assegnato per l'insegnamento di tale corso). E un `Person` entità dispone di una proprietà di navigazione denominata `Courses` che può contenere nessuno, uno o più correlate `Course` entities (che rappresentano i corsi assegnati tale insegnante di insegnare). Un insegnante può insegnare più corsi e un corso può essere impartito da più insegnanti. In questa sezione della procedura dettagliata, è possibile aggiungere e rimuovere le relazioni tra `Person` e `Course` entità aggiornando le proprietà di navigazione delle entità correlate.

Creare una nuova pagina web denominata *InstructorsCourses.aspx* che usa le *Site. master* pagina master e aggiungere il markup seguente per il `Content` controllo denominato `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample5.aspx)]

Questo codice crea un' `EntityDataSource` controllo che consente di recuperare il nome e `PersonID` di `Person` entità per instructors (insegnanti). Oggetto `DropDrownList` è associato ai `EntityDataSource` controllo. Il `DropDownList` controllo consente di specificare un gestore per il `DataBound` evento. Si userà questo gestore a elenchi a discesa databind due che consentono di visualizzare i corsi.

Il markup crea anche il gruppo di controlli da usare per l'assegnazione di un corso all'insegnante selezionato seguente:

- Oggetto `DropDownList` controllo per la selezione di un corso da assegnare. Questo controllo verrà popolato con i corsi che attualmente non sono assegnati all'insegnante selezionato.
- Oggetto `Button` controllo per avviare l'assegnazione.
- Oggetto `Label` controllo per visualizzare un messaggio di errore se l'assegnazione ha esito negativo.

Infine, il markup crea anche un gruppo di controlli da usare per la rimozione di un corso dall'insegnante selezionato.

Nelle *InstructorsCourses.aspx.cs*, aggiungere un tramite istruzione:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample6.cs)]

Aggiungere un metodo per il popolamento di due elenchi a discesa che visualizza i corsi:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample7.cs)]

Questo codice ottiene tutti i corsi dal `Courses` entità imposta e ottiene i corsi del `Courses` proprietà di navigazione del `Person` entità per l'insegnante selezionato. Quindi, determina i corsi assegnati a tale insegnante e gli elenchi a discesa verranno popolate di conseguenza.

Aggiungere un gestore per il `Assign` del pulsante `Click` evento:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample8.cs)]

Questo codice ottiene la `Person` entità per l'insegnante selezionato, ottiene il `Course` entità per il corso selezionato e aggiunge il corso selezionato per il `Courses` proprietà di navigazione dell'insegnante `Person` entità. Quindi lo salva le modifiche apportate al database e di ripopola gli elenchi a discesa scegliere in modo che i risultati possono essere visualizzati immediatamente.

Aggiungere un gestore per il `Remove` del pulsante `Click` evento:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample9.cs)]

Questo codice ottiene la `Person` entità per l'insegnante selezionato, ottiene il `Course` entità per il corso selezionato e rimuove il corso selezionato dalle `Person` dell'entità `Courses` proprietà di navigazione. Quindi lo salva le modifiche apportate al database e di ripopola gli elenchi a discesa scegliere in modo che i risultati possono essere visualizzati immediatamente.

Aggiungere il codice per il `Page_Load` metodo per verificare che i messaggi di errore non sono visibili quando si verifica alcun errore per segnalare e aggiungere i gestori per il `DataBound` e `SelectedIndexChanged` gli eventi dell'elenco a discesa scegliere instructors (insegnanti) per popolare gli elenchi a discesa corsi:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample10.cs)]

Eseguire la pagina.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)

Selezionare un insegnante. Il <strong>assegnare un corso</strong> elenco a discesa elenco Visualizza i corsi non insegna l'insegnante, e il <strong>rimuovere un corso</strong> elenco a discesa verranno visualizzati i corsi che è già assegnato il tipo instructor. Nel <strong>assegnare un corso</strong> sezione, selezionare un corso e quindi fare clic su <strong>assegnare</strong>. Sposta il corso per il <strong>rimuovere un corso</strong> elenco a discesa. Selezionare un corso nel <strong>un corso di rimozione</strong> della sezione e fare clic su <strong>rimuovere</strong><em>.</em> Sposta il corso per il <strong>assegnare un corso</strong> elenco a discesa.

Si sono visto alcuni altri modi per lavorare con i dati correlati. Nell'esercitazione seguente, si apprenderà come utilizzare l'ereditarietà nel modello di dati per migliorare la manutenibilità dell'applicazione.

> [!div class="step-by-step"]
> [Precedente](the-entity-framework-and-aspnet-getting-started-part-4.md)
> [Successivo](the-entity-framework-and-aspnet-getting-started-part-6.md)
