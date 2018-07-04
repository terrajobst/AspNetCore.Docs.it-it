---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
title: Introduzione a Entity Framework 4.0 Database First e ASP.NET 4 Web Form - parte 4 | Microsoft Docs
author: tdykstra
description: L'applicazione web di esempio Contoso University illustra come creare applicazioni Web Form ASP.NET utilizzando Entity Framework. L'applicazione di esempio è...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ceb9e60f-957c-4d25-9331-cc527de96a33
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
msc.type: authoredcontent
ms.openlocfilehash: 9783e30fdabe17958690d178ef95616cb72a8164
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380328"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-4"></a>Introduzione a Entity Framework 4.0 Database First e ASP.NET 4 Web Form - parte 4
====================
da [Tom Dykstra](https://github.com/tdykstra)

> L'applicazione web di esempio Contoso University illustra come creare applicazioni Web Form ASP.NET usando il Entity Framework 4.0 e Visual Studio 2010. Per informazioni sulla serie di esercitazioni, vedere [la prima esercitazione della serie](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data"></a>Utilizzo dei dati correlati

Nell'esercitazione precedente è stato usato il `EntityDataSource` controllo da filtrare, ordinare e raggruppare i dati. In questa esercitazione verrà visualizzare e aggiornare i dati correlati.

Si creerà la pagina instructors (insegnanti) Mostra un elenco degli insegnanti. Quando si seleziona un insegnante, viene visualizzato un elenco dei corsi tenuti da tale insegnante. Quando si seleziona un corso, vengono visualizzati i dettagli per il corso e un elenco di studenti iscritti al corso. È possibile modificare il nome insegnante, data di assunzione e assegnazione di ufficio. L'assegnazione dell'ufficio è un set di entità separati che si accede tramite una proprietà di navigazione.

È possibile collegare dati master per i dati di dettaglio nel markup o nel codice. In questa parte dell'esercitazione, si userà entrambi i metodi.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)

## <a name="displaying-and-updating-related-entities-in-a-gridview-control"></a>Visualizzazione e l'aggiornamento di entità correlate in un controllo GridView

Creare una nuova pagina web denominata *Instructors.aspx* che usa le *Site. master* pagina master e aggiungere il markup seguente per il `Content` controllo denominato `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample1.aspx)]

Questo codice crea un `EntityDataSource` controllo che seleziona instructors (insegnanti) e consente gli aggiornamenti. Il `div` elemento configura markup per il rendering a sinistra, in modo che sia possibile aggiungere una colonna a destra in un secondo momento.

Tra i `EntityDataSource` markup e chiusura `</div>` tag, aggiungere il markup seguente che crea un `GridView` controllo e un `Label` controllo che si userà per i messaggi di errore:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample2.aspx)]

Ciò `GridView` controllo consente la selezione di riga, evidenzia la riga selezionata con un colore di sfondo grigio chiaro e specifica i gestori (che si creerà più avanti) per il `SelectedIndexChanged` e `Updating` eventi. Specifica inoltre `PersonID` per il `DataKeyNames` proprietà, in modo che il valore della chiave della riga selezionata può essere passato a un altro controllo che verrà aggiunto in un secondo momento.

L'ultima colonna contiene l'assegnazione dell'ufficio, che viene archiviato in una proprietà di navigazione del `Person` entità perché proviene da un'entità associata. Si noti che il `EditItemTemplate` elemento specifica `Eval` invece di `Bind`, in quanto il `GridView` controllo non è possibile associare direttamente alle proprietà di navigazione per aggiornarli. È possibile aggiornare l'assegnazione dell'ufficio nel codice. A tale scopo, è necessario un riferimento per il `TextBox` controllo, si sarà ottenere e salvare nel `TextBox` del controllo `Init` evento.

Seguendo la `GridView` controllo è un `Label` controllo utilizzato per i messaggi di errore. Il controllo `Visible` è di proprietà `false`, e lo stato di visualizzazione è stato disattivata, in modo che l'etichetta verrà visualizzata solo quando codice rende visibile in risposta a un errore.

Aprire il *Instructors.aspx.cs* del file e aggiungere quanto segue `using` istruzione:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample3.cs)]

Aggiungere un campo di classe privata immediatamente dopo la dichiarazione del nome di classe parziale che include un riferimento alla casella di testo di assegnazione di ufficio.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample4.cs)]

Aggiungere uno stub per il `SelectedIndexChanged` gestore dell'evento che si aggiungerà codice per un uso successivo. Aggiungere anche un gestore per l'assegnazione dell'ufficio `TextBox` del controllo `Init` eventi in modo che è possibile archiviare un riferimento al `TextBox` controllo. Si userà questo riferimento per ottenere il valore immesso per aggiornare l'entità associata alla proprietà di navigazione.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample5.cs)]

Si userà il `GridView` del controllo `Updating` evento da aggiornare la `Location` proprietà della classe associata `OfficeAssignment` entità. Aggiungere il gestore seguente per il `Updating` evento:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample6.cs)]

Questo codice viene eseguito quando l'utente sceglie **Update** in un `GridView` riga. Il codice Usa LINQ to Entities per recuperare il `OfficeAssignment` entità a cui è associato all'oggetto corrente `Person` entità, utilizzando il `PersonID` della riga selezionata dall'argomento dell'evento.

Il codice accetta quindi una delle azioni seguenti a seconda del valore nel `InstructorOfficeTextBox` controllo:

- Se la casella di testo ha un valore ed è presente alcun `OfficeAssignment` entità da aggiornare, ne crea uno.
- Se la casella di testo ha un valore ed è presente un' `OfficeAssignment` entità, viene aggiornata la `Location` valore della proprietà.
- Se la casella di testo è vuota e un' `OfficeAssignment` entità esiste, viene eliminata l'entità.

Al termine, Salva le modifiche al database. Se si verifica un'eccezione, viene visualizzato un messaggio di errore.

Eseguire la pagina.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)

Fare clic su **modifica** e modificare tutti i campi alle caselle di testo.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)

Modificare uno di questi valori, inclusi **assegnazione dell'ufficio**. Fare clic su **Update** si noterà che le modifiche riflesse nell'elenco.

## <a name="displaying-related-entities-in-a-separate-control"></a>Visualizzare le entità correlate in un controllo separato

Ogni insegnante può tenere uno o più corsi, pertanto si aggiungerà un' `EntityDataSource` controllo e una `GridView` controllo per visualizzare l'elenco di corsi associati a qualunque sia il docente selezionato in insegnanti `GridView` controllo. Per creare un'intestazione e il `EntityDataSource` controllo per le entità di corsi, aggiungere il markup seguente tra il messaggio di errore `Label` controllo e la chiusura `</div>` tag:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample7.aspx)]

Il `Where` parametro contiene il valore della `PersonID` dell'insegnante cui la riga è selezionata nel `InstructorsGridView` controllo. Il `Where` proprietà contiene un comando sub-SELECT che ottiene tutti i relativi `Person` entità da una `Course` dell'entità `People` proprietà di navigazione e seleziona il `Course` solo se entità tra i associato`Person`entità contiene selezionato `PersonID` valore.

Per creare il `GridView` del controllo., aggiungere il markup seguente subito dopo il `CoursesEntityDataSource` controllo (prima della chiusura `</div>` tag):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample8.aspx)]

Perché non viene visualizzato alcun corso se non è selezionato alcun insegnante, un `EmptyDataTemplate` elemento è incluso.

Eseguire la pagina.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)

Selezionare un insegnante che dispone di uno o più corsi assegnati e al corso o corsi visualizzato nell'elenco. (Nota: anche se lo schema del database consente più corsi, nei dati di test forniti con il database non insegnante effettivamente ha più di un corso. È possibile aggiungere corsi per il database manualmente utilizzando il **Esplora Server** finestra o il *CoursesAdd.aspx* pagina nella quale verrà aggiunto in un'esercitazione successiva.)

[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)

Il `CoursesGridView` controllo Mostra solo pochi campi di corsi. Per visualizzare tutti i dettagli per un corso, si userà un `DetailsView` controllo per il corso selezionato dall'utente. Nelle *Instructors.aspx*, aggiungere il markup seguente dopo la chiusura `</div>` tag (assicurarsi di inserire questo markup **dopo** un tag div di chiusura, non preceduto da):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample9.aspx)]

Questo codice crea un' `EntityDataSource` controllo a cui è associato il `Courses` set di entità. Il `Where` proprietà consente di selezionare un corso usando il `CourseID` valore della riga selezionata in corsi `GridView` controllo. Il markup specifica un gestore per il `Selected` evento, che verrà usato in un secondo momento per la visualizzazione dei voti, vale a dire un altro livello inferiore nella gerarchia.

Nelle *Instructors.aspx.cs*, lo stub seguente per creare il `CourseDetailsEntityDataSource_Selected` (metodo). (È possibile compilare questo stub più avanti nell'esercitazione, per il momento, è necessario affinché la pagina verrà compilata ed eseguita).

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample10.cs)]

Eseguire la pagina.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)

Inizialmente non sono disponibili dettagli corso perché non è selezionato alcun corso. Selezionare un insegnante che dispone di un corso assegnato e quindi selezionare un corso per visualizzare i dettagli.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)

## <a name="using-the-entitydatasource-selected-event-to-display-related-data"></a>Utilizzo di EntityDataSource "selezionati" evento per visualizzare i dati correlati

Infine, si desidera visualizzare tutti gli studenti iscritti e i voti corrispondenti per il corso selezionato. A tale scopo, si userà il `Selected` eventi del `EntityDataSource` controllo associato al corso `DetailsView`.

Nelle *Instructors.aspx*, aggiungere il seguente markup dopo la `DetailsView` controllo:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample11.aspx)]

Questo codice crea un `ListView` controllo che visualizza un elenco degli studenti e i voti corrispondenti per il corso selezionato. Viene specificata alcuna origine dati perché si imposterà un metodo databind del controllo nel codice. Il `EmptyDataTemplate` elemento fornisce un messaggio da visualizzare quando non è selezionato alcun corso, in tal caso, non esistono Nessun studenti da visualizzare. Il `LayoutTemplate` elemento crea una tabella HTML per visualizzare l'elenco e il `ItemTemplate` specifica le colonne da visualizzare. L'ID dello studente e il livello degli studenti provengono dal `StudentGrade` entità e il nome di uno studente proviene il `Person` entità che rende disponibili in Entity Framework il `Person` proprietà di navigazione del `StudentGrade` entità.

Nelle *Instructors.aspx.cs*, sostituire la sottoposto a stub orizzontale `CourseDetailsEntityDataSource_Selected` metodo con il codice seguente:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample12.cs)]

L'argomento dell'evento per questo evento fornisce i dati sotto forma di una raccolta, che conterrà zero elementi se non è selezionato o un elemento selezionati se un `Course` entità sia selezionata. Se un `Course` entità è selezionata, il codice Usa il `First` metodo per convertire la raccolta a un singolo oggetto. Quindi, recupera `StudentGrade` entità dalla proprietà di navigazione, li converte in una raccolta e associa il `GradesListView` controllo alla raccolta.

Ciò è sufficiente per visualizzare, gradi da, ma si desidera assicurarsi che il messaggio nel modello di dati vuoto viene visualizzato la prima volta che viene visualizzata la pagina e ogni volta che un corso non è selezionato. A tale scopo, creare il metodo seguente, che verrà chiamata da due posizioni:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample13.cs)]

Questo nuovo metodo da chiamare il `Page_Load` metodo per visualizzare i dati vuoti la prima volta che viene visualizzata la pagina. E chiamarlo dal `InstructorsGridView_SelectedIndexChanged` metodo perché tale evento viene generato quando viene selezionato un insegnante, ovvero nuovi corsi vengono caricati i corsi `GridView` controllo e nessuno è stato ancora selezionato. Di seguito sono le due chiamate:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample14.cs)]

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample15.cs)]

Eseguire la pagina.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)

Selezionare un insegnante che dispone di un corso assegnato e quindi selezionare il corso.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)

Si sono visto alcuni modi per lavorare con i dati correlati. Nell'esercitazione seguente, si apprenderà come aggiungere le relazioni tra entità esistenti, come rimuovere le relazioni e come aggiungere una nuova entità che ha una relazione a un'entità esistente.

> [!div class="step-by-step"]
> [Precedente](the-entity-framework-and-aspnet-getting-started-part-3.md)
> [Successivo](the-entity-framework-and-aspnet-getting-started-part-5.md)
