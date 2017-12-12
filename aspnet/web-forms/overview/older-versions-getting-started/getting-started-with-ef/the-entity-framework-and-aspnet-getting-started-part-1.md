---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
title: Guida introduttiva a Database di Entity Framework 4.0 prima di tutto e ASP.NET 4 Web Form | Documenti Microsoft
author: tdykstra
description: L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni Web Form ASP.NET utilizzando il Entity Framework 4.0 e Visual Studio 2010...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 5cb00916-8f46-491f-be25-4739a615d619
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
msc.type: authoredcontent
ms.openlocfilehash: 5a852ec2301e63bde9a5ce99db80224dad7fb258
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms"></a>Guida introduttiva a Database di Entity Framework 4.0 prima di tutto e ASP.NET 4 Web Form
====================
Da [Tom Dykstra](https://github.com/tdykstra)

> L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni Web Form ASP.NET utilizzando il Entity Framework 4.0 e Visual Studio 2010. L'applicazione di esempio è un sito Web per un'università Contoso fittizio. Include funzionalità, ad esempio l'ammissione di studenti, la creazione di corsi e assegnazioni istruttore.
> 
> L'esercitazione è riportati esempi in c#. Il [esempio scaricabile](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) contiene il codice in c# e Visual Basic.
> 
> ## <a name="database-first"></a>Prima di database
> 
> È possibile utilizzare i dati in Entity Framework in tre modi: *Database First*, *Model First*, e *Code First*. In questa esercitazione è per il primo Database. Per informazioni sulle differenze tra questi flussi di lavoro e indicazioni su come scegliere quello più adatto per lo scenario, vedere [flussi di lavoro di Entity Framework sviluppo](https://msdn.microsoft.com/en-us/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Web Form
> 
> Questa serie di esercitazioni utilizza il modello Web Form ASP.NET e presuppone che si conosca come utilizzare i Web Form ASP.NET in Visual Studio. Se non, viene visualizzato [Introduzione a Web Form ASP.NET 4.5](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Se si preferisce utilizzare con il framework ASP.NET MVC, vedere [Introduzione a Entity Framework usando ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Versioni del software
> 
> | **Nell'esercitazione** | **Funziona anche con** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express per Web. L'esercitazione non è stata testata con le versioni successive di Visual Studio. Esistono numerose differenze in selezioni di menu, finestre di dialogo e i modelli. |
> | .NET 4 | .NET 4.5 è compatibile con .NET 4, ma l'esercitazione non è stata testata con .NET 4.5. |
> | Entity Framework 4 | L'esercitazione non è stata testata con le versioni successive di Entity Framework. A partire da Entity Framework 5, Entity Framework Usa per impostazione predefinita il `DbContext API` che è stata introdotta con Entity Framework 4.1. Il controllo EntityDataSource è stato progettato per utilizzare il `ObjectContext` API. Per informazioni su come usare EntityDataSource controllare con il `DbContext` API, vedere [questo post di blog](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Domande
> 
> Nel caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), [Entity Framework e LINQ to forum entità](https://social.msdn.microsoft.com/forums/en-US/adodotnetentityframework/threads/), o [ StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Panoramica

L'applicazione da generare in queste esercitazioni è un sito Web semplice university.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image1.png)

Gli utenti possono visualizzare e aggiornare studente, corsi e istruttore informazioni. Seguito sono riportate alcune delle schermate che verranno create.

[![Image30](the-entity-framework-and-aspnet-getting-started-part-1/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image3.png)

[![Image37](the-entity-framework-and-aspnet-getting-started-part-1/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image5.png)

[![Image31](the-entity-framework-and-aspnet-getting-started-part-1/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image7.png)

[![Image32](the-entity-framework-and-aspnet-getting-started-part-1/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image9.png)

## <a name="creating-the-web-application"></a>Creazione dell'applicazione Web

Per avviare l'esercitazione, aprire Visual Studio e quindi creare un nuovo progetto applicazione Web ASP.NET utilizzando il **applicazione Web ASP.NET** modello:

[![Image01](the-entity-framework-and-aspnet-getting-started-part-1/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image11.png)

Questo modello consente di creare un progetto di applicazione web che include già un foglio di stile e le pagine master:

[![Image02](the-entity-framework-and-aspnet-getting-started-part-1/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image13.png)

Aprire il *Site. master* file e modificare "Applicazione ASP.NET" in "Contoso University".

[!code-html[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample1.html)]

Trovare il *Menu* controllo denominato `NavigationMenu` e sostituirlo con il markup seguente, che consente di aggiungere voci di menu per le pagine che verrà creato.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample2.aspx)]

Aprire il *Default.aspx* pagina e modificare il `Content` controllo denominato `BodyContent` a questo:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample3.aspx)]

È ora una semplice home page con collegamenti alle varie pagine che verranno creati:

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image15.png)

## <a name="creating-the-database"></a>Creazione del Database

Per queste esercitazioni, si utilizzerà Progettazione modelli di dati Entity Framework per creare automaticamente il modello di dati basato su un database esistente (spesso chiamato il *database-first* approccio). Alternativa che non viene descritta in questa serie di esercitazioni, è possibile creare manualmente il modello di dati e quindi gli script di generazione della finestra di progettazione che crea il database (il *model-first* approccio).

Per il metodo di database-first utilizzato in questa esercitazione, il passaggio successivo è per aggiungere un database per il sito. Il modo più semplice consiste innanzitutto scaricare il progetto da associare a questa esercitazione. Quindi il *App\_dati* cartella, selezionare **Aggiungi elemento esistente**e selezionare il *School. mdf* file di database dal progetto scaricato.

In alternativa è possibile seguire le istruzioni in [la creazione di Database di esempio School](https://msdn.microsoft.com/en-us/library/bb399731.aspx). Se si scarica il database o crearla, copiare il *School. mdf* file dalla cartella dell'applicazione *App\_dati* cartella:

`%PROGRAMFILES%\Microsoft SQL Server\MSSQL10.SQLEXPRESS\MSSQL\DATA`

(Il percorso del *con estensione mdf* file presuppone che si utilizza SQL Server 2008 Express.)

Se si crea il database da uno script, eseguire la procedura seguente per creare un diagramma di database:

1. In **Esplora Server**, espandere **connessioni dati**, espandere *School. mdf*, fare doppio clic su **diagrammi di Database**e selezionare **Aggiungere nuovo diagramma**.

    [![Image35](the-entity-framework-and-aspnet-getting-started-part-1/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image17.png)
2. Selezionare tutte le tabelle e quindi fare clic su **Aggiungi**.

    [![Image36](the-entity-framework-and-aspnet-getting-started-part-1/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image19.png)

    SQL Server crea un diagramma di database contenente tabelle, colonne, tabelle e delle relazioni tra le tabelle. È possibile spostare le tabelle per organizzare le proprie preferenze.
3. Salvare il diagramma come "SchoolDiagram" e chiuderlo.

Se si scarica il *School. mdf* file da associare a questa esercitazione, è possibile visualizzare il diagramma di database facendo doppio clic su **SchoolDiagram** in **diagrammi di Database** in **Esplora server**.

[![Image38](the-entity-framework-and-aspnet-getting-started-part-1/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image21.png)

Il diagramma è simile alla seguente (le tabelle potrebbero essere in posizioni diverse rispetto a quanto mostrato qui):

[![Image04](the-entity-framework-and-aspnet-getting-started-part-1/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image23.png)

## <a name="creating-the-entity-framework-data-model"></a>Creare il modello di dati di Entity Framework

È ora possibile creare un modello di dati di Entity Framework da questo database. È possibile creare il modello di dati nella cartella radice dell'applicazione, ma per questa esercitazione è possibile inserirlo in una cartella denominata *DAL* (per il livello di accesso ai dati).

In **Esplora**, aggiungere una cartella di progetto denominata *DAL* (assicurarsi che sia sotto il progetto, non nella soluzione).

Fare doppio clic su di *DAL* cartella e quindi selezionare **Aggiungi** e **nuovo elemento**. In **modelli installati**selezionare **dati**, selezionare il **ADO.NET Entity Data Model** modello, il nome *SchoolModel*, e quindi fare clic su **Aggiungi**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-1/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image25.png)

Verrà avviata la procedura guidata Entity Data Model. Nel primo passaggio nella procedura guidata, il **genera da database** opzione è selezionata per impostazione predefinita. Scegliere **Avanti**.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-1/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image27.png)

Nel **Seleziona connessione dati** passaggio, lasciare i valori predefiniti e fare clic su **Avanti**. Il database School è selezionato per impostazione predefinita e l'impostazione di connessione viene salvata nel *Web. config* file come **SchoolEntities**.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-1/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image29.png)

Nel **Seleziona oggetti di Database** passaggio della procedura guidata, selezionare tutte le tabelle, ad eccezione `sysdiagrams` (che è stato creato per il diagramma è stato generato in precedenza) e quindi fare clic su **fine**.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-1/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image31.png)

Al termine della creazione del modello, Visual Studio Mostra una rappresentazione grafica degli oggetti Entity Framework (entità) che corrispondono alle tabelle di database. (Come per il diagramma di database, il percorso dei singoli elementi potrebbe essere diverso da quelle visualizzate in questa illustrazione. È possibile trascinare gli elementi per circa corrispondono nella figura, se si desidera.)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-1/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image33.png)

## <a name="exploring-the-entity-framework-data-model"></a>Esplorazione del modello di dati di Entity Framework

È possibile vedere che il diagramma di entità è molto simile al diagramma di database, con alcune differenze. Una differenza consiste nell'aggiunta dei simboli alla fine di ogni associazione che indicano il tipo di associazione (le relazioni tra tabelle vengono chiamate le associazioni di entità nel modello di dati):

- Un'associazione uno-a-zero-o-uno è rappresentata da "1" e "0..1".

    [![Image39](the-entity-framework-and-aspnet-getting-started-part-1/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image35.png)

    In questo caso, un `Person` entità può o non può essere associata a un `OfficeAssignment` entità. Un `OfficeAssignment` entità deve essere associato un `Person` entità. In altre parole, un docente può o non può essere assegnato a un ufficio, e un ufficio può essere assegnato a solo un insegnante.
- Un'associazione uno-a-molti è rappresentata da "1" e "\*".

    [![Image40](the-entity-framework-and-aspnet-getting-started-part-1/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image37.png)

    In questo caso, un `Person` entità può o non possano essere associate `StudentGrade` entità. Oggetto `StudentGrade` entità deve essere associata a uno `Person` entità. `StudentGrade`le entità rappresentano effettivamente corsi registrati in questo database. Se uno studente viene registrato un corso ed è presente alcun livello, il `Grade` proprietà è null. In altre parole, uno studente non può essere registrato in qualsiasi corsi ancora, può essere registrato in uno corso, oppure può essere registrato in più corsi. Ciascun anno scolastico di un corso registrato si applica a un solo studente.
- Un'associazione molti-a-molti è rappresentata da "\*"e"\*".

    [![Image41](the-entity-framework-and-aspnet-getting-started-part-1/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image39.png)

    In questo caso, un `Person` entità può o non possano essere associate `Course` entità e viceversa è true: un `Course` entità può o non possano essere associate `Person` entità. In altre parole, un docente potrebbe indicare più corsi e un corso può dedicare da istruttori più. (In questo database, questa relazione è valida solo per i docenti; non è collegato studenti ai corsi. Gli studenti sono collegati ai corsi dalla tabella StudentGrades.)

Un'altra differenza tra il diagramma di database e il modello di dati è aggiuntiva **le proprietà di navigazione** sezione per ogni entità. Una proprietà di navigazione di un'entità fa riferimento a entità correlate. Ad esempio, il `Courses` proprietà in un `Person` entità contiene una raccolta di tutti i `Course` entità correlate che `Person` entità.

[![Image12](the-entity-framework-and-aspnet-getting-started-part-1/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image41.png)

Un'altra differenza tra il modello di dati e il database è l'assenza del `CourseInstructor` tabella di associazione che viene utilizzata nel database per collegare il `Person` e `Course` tabelle in una relazione molti-a-molti. Le proprietà di navigazione consentono di ottenere correlati `Course` entità dal `Person` entità ed elementi correlati `Person` entità dal `Course` entità, in modo non è necessario per rappresentare la tabella di associazione del modello di dati.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-1/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image43.png)

Ai fini di questa esercitazione, si supponga che il `FirstName` colonna il `Person` tabella effettivamente contiene sia una persona nome e cognome. Si desidera modificare il nome del campo in modo da riflettere questo, ma l'amministratore del database (DBA) potrebbe non voler modificare il database. È possibile modificare il nome della `FirstName` proprietà nel modello di dati, lasciando il database equivalente invariata.

Nella finestra di progettazione, fare doppio clic su **FirstName** nel `Person` entità e quindi selezionare **rinominare**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-1/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image45.png)

Digitare il nuovo nome "FirstMidName". Questa modifica il modo in cui che si fa riferimento alla colonna nel codice senza modificare il database.

[![Image29](the-entity-framework-and-aspnet-getting-started-part-1/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image47.png)

Il browser modello fornisce un altro modo per visualizzare la struttura del database, la struttura del modello di dati e il mapping tra di essi. Per visualizzarlo, fare doppio clic su un'area vuota nella finestra di progettazione entità e quindi fare clic su **Browser modello**.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-1/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image49.png)

Il **Browser modello** riquadro contiene una visualizzazione albero. (Il **Browser modello** riquadro può essere ancorato con il **Esplora** riquadro.) Il **SchoolModel** nodo rappresenta la struttura del modello di dati e **SchoolModel.Store** nodo rappresenta la struttura del database.

[![Image26](the-entity-framework-and-aspnet-getting-started-part-1/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image51.png)

Espandere **SchoolModel.Store** per visualizzare le tabelle, espandere **tabelle / viste** per visualizzare le tabelle e quindi espandere **corso** per visualizzare le colonne all'interno di una tabella.

[![Image19](the-entity-framework-and-aspnet-getting-started-part-1/_static/image54.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image53.png)

Espandere **SchoolModel**, espandere **tipi di entità**, quindi espandere il **corso** nodo per visualizzare le entità e le proprietà nelle entità.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-1/_static/image56.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image55.png)

In una finestra di progettazione o **Browser modello** riquadro è possibile visualizzare la relazione tra gli oggetti dei due modelli di Entity Framework. Fare doppio clic su di `Person` entità e selezionare **tabella Mapping**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-1/_static/image58.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image57.png)

Verrà visualizzata la **Dettagli Mapping** finestra. Si noti che questa finestra consente di verificare che la colonna di database `FirstName` viene eseguito il mapping a `FirstMidName`, che è ciò che è stato rinominato nel modello di dati.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-1/_static/image60.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image59.png)

Entity Framework Usa XML per archiviare le informazioni relative al database, il modello di dati e i mapping tra di essi. Il *SchoolModel* file è effettivamente un file XML che contiene queste informazioni. La finestra di progettazione esegue il rendering le informazioni in formato grafico, ma è anche possibile visualizzare il file in formato XML facendo clic con il *edmx* file in **Esplora**, scegliendo **Apri con**e selezionando **Editor XML (testo)**. (Progettazione modelli di dati e un editor XML sono solo due diversi modi di apertura e l'utilizzo con lo stesso file, in modo da aprire e aprire il file in un editor XML nello stesso momento, non è possibile avere la finestra di progettazione).

È stato creato un sito Web, un database e un modello di dati. Nella procedura dettagliata successiva verrà procedere con i dati utilizzando il modello di dati e ASP.NET `EntityDataSource` controllo.

>[!div class="step-by-step"]
[Successivo](the-entity-framework-and-aspnet-getting-started-part-2.md)
