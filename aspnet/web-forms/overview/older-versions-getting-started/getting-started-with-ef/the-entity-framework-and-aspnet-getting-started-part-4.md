---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
title: Guida introduttiva a Database di Entity Framework 4.0 prima di tutto e ASP.NET 4 di Web Form - parte 4 | Documenti Microsoft
author: tdykstra
description: L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni Web Form ASP.NET tramite Entity Framework. È l'applicazione di esempio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ceb9e60f-957c-4d25-9331-cc527de96a33
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
msc.type: authoredcontent
ms.openlocfilehash: 6bea5f4faeb0a9c11a406a7e4e87c4929eda6a18
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-4"></a>Guida introduttiva a Database di Entity Framework 4.0 prima di tutto e form ASP.NET Web 4 - parte 4
====================
da [Tom Dykstra](https://github.com/tdykstra)

> L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni Web Form ASP.NET utilizzando il Entity Framework 4.0 e Visual Studio 2010. Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data"></a>Utilizzo di dati correlati

Nell'esercitazione precedente è stata usata la `EntityDataSource` controllo da filtrare, ordinare e raggruppare i dati. In questa esercitazione verrà visualizzare e aggiornare i dati correlati.

Creerai la pagina istruttori che mostra un elenco di istruttori. Quando si seleziona un docente, vedrai un elenco di corsi tenuti da tale istruttore. Quando si seleziona un corso, vedere i dettagli per la linea e un elenco di studenti iscritti in corso. È possibile modificare il nome di istruttore, data di assunzione e assegnazione di office. L'assegnazione di office è un set di entità distinta che si accede tramite una proprietà di navigazione.

Dati di dettaglio nel markup o nel codice, è possibile collegare dati master. In questa parte dell'esercitazione, si userà entrambi i metodi.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)

## <a name="displaying-and-updating-related-entities-in-a-gridview-control"></a>Visualizzazione e aggiornamento di entità correlate in un controllo GridView

Creare una nuova pagina web denominata *Instructors.aspx* che utilizza il *Site. master* pagina master e aggiungere il markup seguente per il `Content` controllo denominato `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample1.aspx)]

Questo codice crea un `EntityDataSource` controllo che seleziona i docenti e consente gli aggiornamenti. Il `div` elemento configura markup per il rendering a sinistra, in modo che è possibile aggiungere una colonna a destra in un secondo momento.

Tra le `EntityDataSource` markup e chiusura `</div>` tag, aggiungere il markup seguente che crea un `GridView` controllo e un `Label` controllo che verrà utilizzato per i messaggi di errore:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample2.aspx)]

Questo `GridView` controllo consente la selezione di riga, evidenziare la riga selezionata con un colore di sfondo grigio chiaro e specifica i gestori (che verrà creato in un secondo momento) per il `SelectedIndexChanged` e `Updating` eventi. Specifica inoltre `PersonID` per il `DataKeyNames` proprietà, in modo che il valore della chiave della riga selezionata può essere passato a un altro controllo che verrà aggiunto in un secondo momento.

L'ultima colonna contiene l'assegnazione dell'ufficio dell'insegnante, che viene archiviato in una proprietà di navigazione del `Person` entità perché proviene da un'entità associata. Si noti che il `EditItemTemplate` elemento specifica `Eval` anziché `Bind`, in quanto il `GridView` controllo non è possibile associare direttamente alle proprietà di navigazione per aggiornarli. Si aggiornerà l'assegnazione di office nel codice. A tale scopo, è necessario un riferimento di `TextBox` controllo e si verrà ottenere e salvare nel `TextBox` del controllo `Init` evento.

Dopo il `GridView` controllo è un `Label` controllo utilizzato per i messaggi di errore. Il controllo `Visible` proprietà `false`, e lo stato di visualizzazione è disattivato, in modo che l'etichetta verrà visualizzata solo quando codice rende visibile in risposta a un errore.

Aprire il *Instructors.aspx.cs* file e aggiungere le seguenti `using` istruzione:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample3.cs)]

Aggiungere un campo di classe privata immediatamente dopo la dichiarazione del nome di classe parziale per mantenere un riferimento alla casella di testo di assegnazione di office.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample4.cs)]

Aggiungere uno stub per il `SelectedIndexChanged` gestore dell'evento che verrà aggiunto il codice per un momento successivo. Aggiungere anche un gestore per l'assegnazione di office `TextBox` del controllo `Init` evento in modo che è possibile archiviare un riferimento al `TextBox` controllo. Utilizzare questo riferimento per ottenere il valore immesso per aggiornare l'entità associata alla proprietà di navigazione.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample5.cs)]

Si userà il `GridView` del controllo `Updating` evento per l'aggiornamento di `Location` proprietà dell'oggetto associato `OfficeAssignment` entità. Aggiungere il seguente gestore per il `Updating` evento:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample6.cs)]

Questo codice viene eseguito quando l'utente fa clic **aggiornamento** in un `GridView` riga. Il codice Usa LINQ to Entities per recuperare il `OfficeAssignment` entità associata all'oggetto corrente `Person` entità, utilizzando il `PersonID` della riga selezionata da argomento dell'evento.

Il codice viene quindi una delle azioni seguenti a seconda del valore nel `InstructorOfficeTextBox` controllo:

- Se la casella di testo ha un valore e non esiste alcun `OfficeAssignment` entità da aggiornare, viene creato uno.
- Se la casella di testo ha un valore e non esiste un `OfficeAssignment` entità, viene aggiornato il `Location` valore della proprietà.
- Se la casella di testo è vuota e un `OfficeAssignment` entità esiste, viene eliminata l'entità.

Al termine, Salva le modifiche al database. Se si verifica un'eccezione, viene visualizzato un messaggio di errore.

Eseguire la pagina.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)

Fare clic su **modifica** e modificare tutti i campi alle caselle di testo.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)

Modificare uno di questi valori, tra cui **Office assegnazione**. Fare clic su **aggiornamento** si noterà che le modifiche riflesse nell'elenco.

## <a name="displaying-related-entities-in-a-separate-control"></a>La visualizzazione delle entità correlate in un controllo separato

Ogni istruttore insegnare uno o più corsi, pertanto si aggiungerà un `EntityDataSource` controllo e un `GridView` controllo per elencare i corsi associati a seconda del valore istruttore è selezionata in istruttori `GridView` controllo. Per creare un'intestazione e il `EntityDataSource` di controllo per le entità corsi, aggiungere il markup seguente tra il messaggio di errore `Label` controllo e la chiusura `</div>` tag:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample7.aspx)]

Il `Where` parametro contiene il valore della `PersonID` di istruttore a cui la riga è selezionata nel `InstructorsGridView` controllo. Il `Where` proprietà contiene un comando sub-SELECT che ottiene tutti i relativi `Person` entità da un `Course` dell'entità `People` proprietà di navigazione e seleziona il `Course` solo se entità un l'associato `Person`entità contiene selezionato `PersonID` valore.

Per creare il `GridView` controllo. aggiungere il markup seguente immediatamente dopo il `CoursesEntityDataSource` controllo (prima della chiusura `</div>` tag):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample8.aspx)]

Poiché nessun corso verrà visualizzato se non è selezionata alcuna istruttore, un `EmptyDataTemplate` elemento viene incluso.

Eseguire la pagina.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)

Selezionare un docente ha assegnati a uno o più corsi e il corso o i corsi vengono visualizzati nell'elenco. (Nota: anche se lo schema del database consente più corsi, nei dati di test forniti con il database non istruttore abbia effettivamente più di una linea. È possibile aggiungere i corsi per il database manualmente utilizzando il **Esplora Server** finestra o *CoursesAdd.aspx* pagina nella quale verrà aggiunto in un'esercitazione successiva.)

[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)

Il `CoursesGridView` controllo Mostra solo alcuni campi corso. Per visualizzare tutti i dettagli per un corso, si userà un `DetailsView` controllo per il corso selezionato dall'utente. In *Instructors.aspx*, aggiungere il markup seguente dopo la chiusura `</div>` tag (assicurarsi di inserire questo markup **dopo** tag div di chiusura, non prima che):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample9.aspx)]

Questo codice crea un `EntityDataSource` controllo a cui è associato il `Courses` set di entità. Il `Where` proprietà consente di selezionare un corso usando il `CourseID` valore della riga selezionata nei corsi `GridView` controllo. Il markup specifica un gestore per il `Selected` evento, che verranno usati in un secondo momento per la visualizzazione dei voti, che è un altro livello inferiore nella gerarchia.

In *Instructors.aspx.cs*, creare lo stub seguente per il `CourseDetailsEntityDataSource_Selected` metodo. (Si riempirà questo stub out in un secondo momento nell'esercitazione, per il momento, è necessario in modo che la pagina verrà compilata ed eseguita).

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample10.cs)]

Eseguire la pagina.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)

Non sono inizialmente disponibili dettagli corso perché nessun corso è selezionato. Selezionare un docente che dispone di un corso assegnato e quindi selezionare un corso per visualizzare i dettagli.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)

## <a name="using-the-entitydatasource-selected-event-to-display-related-data"></a>Utilizzo di EntityDataSource "selezionati" evento per visualizzare dati correlati

Infine, si desidera visualizzare tutti gli studenti registrati e i relativi voti per il corso selezionato. A tale scopo, si userà il `Selected` evento del `EntityDataSource` controllo associato al corso `DetailsView`.

In *Instructors.aspx*, aggiungere il markup seguente dopo il `DetailsView` controllo:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample11.aspx)]

Questo codice crea un `ListView` controllo che visualizza un elenco di studenti e i relativi voti per il corso selezionato. Viene specificata alcuna origine dati perché si sarà databind del controllo nel codice. Il `EmptyDataTemplate` elemento fornisce un messaggio da visualizzare quando non viene selezionato alcun corso, in tal caso, non esistono alcun studenti da visualizzare. Il `LayoutTemplate` elemento crea una tabella HTML per visualizzare l'elenco e `ItemTemplate` specifica le colonne da visualizzare. L'ID dello studente e il livello di studenti sono compresi il `StudentGrade` entità e il nome di uno studente è dal `Person` entità che rende disponibili in Entity Framework il `Person` proprietà di navigazione del `StudentGrade` entità.

In *Instructors.aspx.cs*, sostituire sottoposto a stub la `CourseDetailsEntityDataSource_Selected` (metodo) con il codice seguente:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample12.cs)]

L'argomento dell'evento per questo evento fornisce i dati selezionati nel formato di una raccolta, che conterrà zero elementi se non è selezionato alcun elemento o un elemento se un `Course` entità selezionata. Se un `Course` entità è selezionata, il codice Usa il `First` metodo per convertire la raccolta a un singolo oggetto. Ottiene quindi `StudentGrade` entità dalla proprietà di navigazione, li converte in una raccolta e associa il `GradesListView` controllo alla raccolta.

Ciò è sufficiente per la visualizzazione di qualità, ma si desidera assicurarsi che il messaggio nel modello di dati vuoto viene visualizzato la prima volta che viene visualizzata la pagina e ogni volta che un corso non è selezionato. A tale scopo, creare il metodo seguente, che verrà chiamato da due posizioni:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample13.cs)]

Questo nuovo metodo da chiamare il `Page_Load` metodo per visualizzare i dati vuoti la prima volta che viene visualizzata la pagina. E chiamarlo dal `InstructorsGridView_SelectedIndexChanged` metodo poiché tale evento viene generato quando viene selezionato un docente, ovvero nuovi corsi vengono caricati i corsi `GridView` controllo e non è stato ancora selezionato. Di seguito sono le due chiamate:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample14.cs)]

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample15.cs)]

Eseguire la pagina.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)

Selezionare un docente che dispone di un corso assegnato e quindi selezionare la linea.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)

Sono stati analizzati alcuni modi per lavorare con i dati correlati. Nell'esercitazione seguente, si apprenderà come aggiungere le relazioni tra le entità esistenti, come rimuovere le relazioni e come aggiungere una nuova entità che ha una relazione a un'entità esistente.

> [!div class="step-by-step"]
> [Precedente](the-entity-framework-and-aspnet-getting-started-part-3.md)
> [Successivo](the-entity-framework-and-aspnet-getting-started-part-5.md)
