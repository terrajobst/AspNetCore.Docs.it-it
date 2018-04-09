---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
title: Guida introduttiva a Database di Entity Framework 4.0 prima di tutto e ASP.NET 4 di Web Form - parte 5 | Documenti Microsoft
author: tdykstra
description: L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni Web Form ASP.NET tramite Entity Framework. È l'applicazione di esempio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 24ad4379-3fb2-44dc-ba59-85fe0ffcb2ae
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
msc.type: authoredcontent
ms.openlocfilehash: 7f351718123e881e832f4ac95af506ed601d3337
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-5"></a>Guida introduttiva a Database di Entity Framework 4.0 prima di tutto e form ASP.NET Web 4 - parte 5
====================
da [Tom Dykstra](https://github.com/tdykstra)

> L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni Web Form ASP.NET utilizzando il Entity Framework 4.0 e Visual Studio 2010. Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data-continued"></a>Utilizzo di dati correlati, continua

Nell'esercitazione precedente si è iniziato a usare il `EntityDataSource` per funzionare con i dati correlati. È visualizzata più livelli della gerarchia e modificare i dati nelle proprietà di navigazione. In questa esercitazione è possibile continuare a lavorare con i dati correlati aggiunta ed eliminazione di relazioni e aggiungendo una nuova entità che ha una relazione a un'entità esistente.

Si creerà una pagina che aggiunge corsi assegnati ai reparti. I reparti esistono già, e quando si crea un nuovo corso, nello stesso momento verrà stabilire una relazione tra i file e una categoria esistente.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)

Verrà creata anche una pagina che funziona con una relazione molti-a-molti tramite l'assegnazione di un docente a un corso (aggiunta di una relazione tra due entità che si seleziona) o la rimozione di un docente da un corso (rimozione di una relazione tra due entità che si Selezionare). Nel database, l'aggiunta di una relazione tra un docente e un corso determina una nuova riga viene aggiunta al `CourseInstructor` tabella associazione, la rimozione di una relazione comporta l'eliminazione di una riga dal `CourseInstructor` tabella associazione. Tuttavia, eseguire questa operazione in Entity Framework impostando le proprietà di navigazione, senza fare riferimento al `CourseInstructor` tabella in modo esplicito.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)

## <a name="adding-an-entity-with-a-relationship-to-an-existing-entity"></a>Aggiunta di un'entità con una relazione a un'entità esistente

Creare una nuova pagina web denominata *CoursesAdd.aspx* che utilizza il *Site. master* pagina master e aggiungere il markup seguente per il `Content` controllo denominato `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample1.aspx)]

Questo codice crea un `EntityDataSource` controllo che seleziona i corsi, che consente di inserire e specifica un gestore per il `Inserting` evento. Si userà il gestore per l'aggiornamento di `Department` proprietà di navigazione quando un nuovo `Course` creazione dell'entità.

Il codice crea inoltre un `DetailsView` controllo da utilizzare per l'aggiunta di nuove `Course` entità. Il markup utilizza campi associati per `Course` proprietà dell'entità. È necessario immettere il `CourseID` valore perché non è un campo ID generato dal sistema. In alternativa, è un numero di corso che deve essere specificato manualmente dopo aver creato il corso.

Si utilizza un modello di campo per il `Department` proprietà di navigazione perché le proprietà di navigazione possono essere utilizzate con `BoundField` controlli. Il campo modello fornisce un elenco di riepilogo a discesa per selezionare il reparto. L'elenco a discesa è associato al `Departments` entità impostata per l'utilizzo `Eval` anziché `Bind`, nuovamente perché non è possibile associare direttamente le proprietà di navigazione per aggiornarli. Specificare un gestore per il `DropDownList` del controllo `Init` evento in modo che è possibile archiviare un riferimento al controllo per l'utilizzo dal codice che aggiorna il `DepartmentID` chiave esterna.

In *CoursesAdd.aspx.cs* subito dopo la dichiarazione di classe parziale, aggiungere un campo di classe per contenere un riferimento al `DepartmentsDropDownList` controllo:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample2.cs)]

Aggiungere un gestore per il `DepartmentsDropDownList` del controllo `Init` eventi in modo che è possibile archiviare un riferimento al controllo. Ciò consente di ottenere il valore a cui l'utente ha immesso e utilizzarla per aggiornare il `DepartmentID` valore il `Course` entità.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample3.cs)]

Aggiungere un gestore per il `DetailsView` del controllo `Inserting` evento:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample4.cs)]

Quando l'utente fa clic `Insert`, `Inserting` evento viene generato prima dell'inserimento del nuovo record. Ottiene il codice nel gestore di `DepartmentID` dal `DropDownList` controllare e viene utilizzata per impostare il valore che verrà utilizzato per il `DepartmentID` proprietà del `Course` entità.

Entity Framework si occuperà di questo corso di aggiunta di `Courses` proprietà di navigazione della classe associata `Department` entità. Aggiunge inoltre il reparto di `Department` proprietà di navigazione del `Course` entità.

Eseguire la pagina.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)

Immettere un ID, titolo, un numero di crediti e selezionare un reparto, quindi fare clic su **inserire**.

Eseguire il *Courses.aspx* pagina e selezionare per visualizzare il nuovo corso stesso reparto.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)

## <a name="working-with-many-to-many-relationships"></a>Utilizzo delle relazioni molti-a-molti

La relazione tra il `Courses` set di entità e `People` set di entità è una relazione molti-a-molti. Oggetto `Course` entità dispone di una proprietà di navigazione denominata `People` che può contenere zero, uno o più correlate `Person` entità (che rappresentano i docenti assegnati gli quel corso). E un `Person` entità dispone di una proprietà di navigazione denominata `Courses` che può contenere zero, uno o più correlate `Course` entità (che rappresentano i corsi che istruttore viene assegnato a illustrare). Un insegnante potrebbe indicare più corsi e un corso può dedicare da istruttori più. In questa sezione della procedura dettagliata, aggiungerai e rimuovere le relazioni tra `Person` e `Course` entità aggiornando le proprietà di navigazione di entità correlate.

Creare una nuova pagina web denominata *InstructorsCourses.aspx* che utilizza il *Site. master* pagina master e aggiungere il markup seguente per il `Content` controllo denominato `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample5.aspx)]

Questo codice crea un `EntityDataSource` controllo che recupera il nome e `PersonID` di `Person` entità per i docenti. Oggetto `DropDrownList` è associato al `EntityDataSource` controllo. Il `DropDownList` controllo specifica un gestore per il `DataBound` evento. Si utilizzerà questo gestore a elenchi a discesa databind due che consentono di visualizzare i corsi.

Il codice crea inoltre il gruppo di controlli da usare per l'assegnazione di un corso a istruttore selezionato seguente:

- Oggetto `DropDownList` controllo per la selezione di un corso da assegnare. Questo controllo verrà popolato con i corsi che non sono attualmente assegnati a istruttore selezionato.
- Oggetto `Button` controllo per avviare l'assegnazione.
- Oggetto `Label` viene visualizzato un messaggio di errore se l'assegnazione ha esito negativo.

Infine, il codice crea inoltre un gruppo di controlli da usare per la rimozione di un corso da istruttore selezionato.

In *InstructorsCourses.aspx.cs*, aggiungere un tramite istruzione:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample6.cs)]

Aggiungere un metodo per il popolamento di due elenchi a discesa che Visualizza corsi:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample7.cs)]

Il codice ottiene tutti i corsi dal `Courses` set di entità e ottiene i corsi dal `Courses` proprietà di navigazione del `Person` entità per istruttore selezionato. Quindi, determina quali corsi vengono assegnati a tale instructor e popola di conseguenza gli elenchi a discesa.

Aggiungere un gestore per il `Assign` del pulsante `Click` evento:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample8.cs)]

Il codice ottiene la `Person` entità per istruttore selezionato, ottiene il `Course` entità per il corso selezionato e lo aggiunge al corso selezionato per il `Courses` proprietà di navigazione di un istruttore `Person` entità. Quindi Salva le modifiche apportate al database e ripopolare gli elenchi a discesa, pertanto i risultati possono essere visti immediatamente.

Aggiungere un gestore per il `Remove` del pulsante `Click` evento:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample9.cs)]

Il codice ottiene la `Person` entità per istruttore selezionato, ottiene il `Course` entità per il corso selezionato e rimuove il corso selezionato dal `Person` dell'entità `Courses` proprietà di navigazione. Quindi Salva le modifiche apportate al database e ripopolare gli elenchi a discesa, pertanto i risultati possono essere visti immediatamente.

Aggiungere codice per il `Page_Load` metodo per verificare che i messaggi di errore non sono visibili quando non si verifica alcun errore per segnalare e aggiungere i gestori per il `DataBound` e `SelectedIndexChanged` eventi dell'elenco di riepilogo a discesa i docenti per popolare gli elenchi a discesa corsi:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample10.cs)]

Eseguire la pagina.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)

Selezionare un docente. Il <strong>assegnare un corso</strong> elenco a discesa Visualizza i corsi istruttore non spiega, e <strong>rimuovere un corso</strong> elenco a discesa Visualizza i corsi istruttore è già assegnato a. Nel <strong>assegnare un corso</strong> sezione, selezionare un corso e quindi fare clic su <strong>assegnare</strong>. Sposta la linea di <strong>rimuovere un corso</strong> elenco a discesa. Selezionare un corso nel <strong>rimuovere una linea di condotta</strong> sezione e fare clic su <strong>rimuovere</strong><em>.</em> Sposta la linea di <strong>assegnare un corso</strong> elenco a discesa.

Sono stati analizzati alcuni altri modi per lavorare con i dati correlati. Nell'esercitazione seguente, si apprenderà come usare l'ereditarietà nel modello di dati per migliorare la manutenibilità dell'applicazione.

> [!div class="step-by-step"]
> [Precedente](the-entity-framework-and-aspnet-getting-started-part-4.md)
> [Successivo](the-entity-framework-and-aspnet-getting-started-part-6.md)
