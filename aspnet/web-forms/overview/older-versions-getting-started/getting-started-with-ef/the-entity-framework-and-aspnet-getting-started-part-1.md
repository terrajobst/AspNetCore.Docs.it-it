---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
title: Introduzione a Entity Framework 4.0 Database First e ASP.NET 4 Web Form | Microsoft Docs
author: tdykstra
description: L'applicazione web di esempio Contoso University illustra come creare applicazioni Web Form ASP.NET con l'Entity Framework 4.0 e Visual Studio 2010...
ms.author: aspnetcontent
ms.date: 12/03/2010
ms.assetid: 5cb00916-8f46-491f-be25-4739a615d619
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
msc.type: authoredcontent
ms.openlocfilehash: b5c2ad33452cacdc1d3aca99c13b8c51e8b58b7c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810576"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms"></a>Introduzione a Entity Framework 4.0 Database First e ASP.NET 4 Web Form
====================
da [Tom Dykstra](https://github.com/tdykstra)

> L'applicazione web di esempio Contoso University illustra come creare applicazioni Web Form ASP.NET usando il Entity Framework 4.0 e Visual Studio 2010. L'applicazione di esempio è un sito Web per una fittizia Contoso University. Include funzionalità, come ad esempio l'ammissione di studenti, la creazione di corsi e le assegnazioni di insegnati.
> 
> L'esercitazione illustra alcuni esempi in c#. Il [esempio scaricabile](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) contiene il codice in c# e Visual Basic.
> 
> ## <a name="database-first"></a>Prima di tutto di database
> 
> Esistono tre modi, è possibile utilizzare i dati in Entity Framework: *Database First*, *Model First*, e *Code First*. Questa esercitazione è destinata prima di Database. Per informazioni sulle differenze tra questi flussi di lavoro e linee guida su come scegliere quello più adatto per lo scenario, vedere [flussi di lavoro sviluppo di Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Web Form
> 
> Questa serie di esercitazioni viene utilizzato il modello Web Form ASP.NET e si presuppone che si sappia usare con Web Form ASP.NET in Visual Studio. Se non, viene visualizzato [Introduzione a Web Form ASP.NET 4.5](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Se si preferisce usare il framework ASP.NET MVC, vedere [Introduzione a Entity Framework con MVC ASP.NET](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Versioni del software
> 
> | **Illustrato nell'esercitazione** | **Si integra inoltre con** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express per Web. L'esercitazione non è stata testata con le versioni successive di Visual Studio. Esistono numerose differenze nelle opzioni del menu, finestre di dialogo e modelli. |
> | .NET 4 | .NET 4.5 è compatibile con .NET 4, ma non l'esercitazione è stata testata con .NET 4.5. |
> | Entity Framework 4 | L'esercitazione non è stata testata con le versioni successive di Entity Framework. A partire da Entity Framework 5, Entity Framework Usa per impostazione predefinita il `DbContext API` che è stata introdotta con Entity Framework 4.1. Il controllo EntityDataSource è stato progettato per utilizzare il `ObjectContext` API. Per informazioni su come usare EntityDataSource controllare con il `DbContext` API, vedere [questo post di blog](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Domande
> 
> Se hai domande che non sono direttamente correlate con l'esercitazione, è possibile pubblicarli per i [forum di ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), il [Entity Framework e LINQ al forum su entità](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), o [ StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Panoramica

L'applicazione che sarà compilata in queste esercitazioni è un sito Web semplice university.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image1.png)

Gli utenti possono visualizzare e aggiornare le informazioni che riguardano studenti, corsi e insegnanti. Seguito sono riportate alcune delle schermate di cui che verrà creato.

[![Image30](the-entity-framework-and-aspnet-getting-started-part-1/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image3.png)

[![Image37](the-entity-framework-and-aspnet-getting-started-part-1/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image5.png)

[![Image31](the-entity-framework-and-aspnet-getting-started-part-1/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image7.png)

[![Image32](the-entity-framework-and-aspnet-getting-started-part-1/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image9.png)

## <a name="creating-the-web-application"></a>Creazione dell'applicazione Web

Per avviare l'esercitazione, aprire Visual Studio e quindi creare un nuovo progetto di applicazione Web ASP.NET usando il **applicazione Web ASP.NET** modello:

[![Image01](the-entity-framework-and-aspnet-getting-started-part-1/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image11.png)

Questo modello crea un progetto di applicazione web che include già un foglio di stile e pagine master:

[![Image02](the-entity-framework-and-aspnet-getting-started-part-1/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image13.png)

Aprire il *Site. master* di file e modificare "My ASP.NET Application" in "Contoso University".

[!code-html[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample1.html)]

Trovare il *dal Menu* controllo denominato `NavigationMenu` e sostituirlo con il markup seguente, che consente di aggiungere voci di menu per le pagine che verrà creato.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample2.aspx)]

Aprire il *default. aspx* pagina e modificare il `Content` controllo denominato `BodyContent` al seguente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample3.aspx)]

A questo punto si dispone di una semplice home page con collegamenti alle varie pagine che verrà creata:

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image15.png)

## <a name="creating-the-database"></a>Creazione del Database

Per queste esercitazioni, si userà Entity Framework data model designer per creare automaticamente il modello di dati basato su un database esistente (spesso denominato il *database-first* approccio). Un'alternativa che non è illustrata in questa serie di esercitazioni consiste nel creare manualmente il modello di dati e hanno quindi gli script di progettazione di generare che crea il database (il *model-first* approccio).

Per il metodo di database-first usato in questa esercitazione, il passaggio successivo consiste nell'aggiungere un database per il sito. Il modo più semplice consiste nello scaricare innanzitutto il progetto da associare a questa esercitazione. Quindi scegliere il *App\_dati* cartella, selezionare **Aggiungi elemento esistente**e selezionare il *School. mdf* file di database dal progetto scaricato.

In alternativa è possibile seguire le istruzioni in [creazione del Database di esempio School](https://msdn.microsoft.com/library/bb399731.aspx). Se si scarica il database o crearla, copiare il *School. mdf* file dalla cartella dell'applicazione *App\_dati* cartella:

`%PROGRAMFILES%\Microsoft SQL Server\MSSQL10.SQLEXPRESS\MSSQL\DATA`

(Questo percorso dei *mdf* file presuppone che si usa SQL Server 2008 Express.)

Se si crea il database da uno script, seguire la procedura seguente per creare un diagramma di database:

1. Nelle **Esplora Server**, espandere **connessioni dati**, espandere *School. mdf*, fare doppio clic su **diagrammi di Database**e scegliere **Aggiungi nuovo diagramma**.

    [![Image35](the-entity-framework-and-aspnet-getting-started-part-1/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image17.png)
2. Selezionare tutte le tabelle e quindi fare clic su **Add**.

    [![Image36](the-entity-framework-and-aspnet-getting-started-part-1/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image19.png)

    SQL Server crea un diagramma di database contenente tabelle, colonne, tabelle e delle relazioni tra le tabelle. È possibile spostare le tabelle per organizzarli desiderato.
3. Salvare il diagramma come "SchoolDiagram" e chiuderlo.

Se si scarica il *School. mdf* file da associare a questa esercitazione, è possibile visualizzare il diagramma di database facendo doppio clic su **SchoolDiagram** sotto **diagrammi di Database** in **Esplora server**.

[![Image38](the-entity-framework-and-aspnet-getting-started-part-1/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image21.png)

Il diagramma simile al seguente (le tabelle potrebbero essere in posizioni diverse rispetto a quanto illustrato di seguito):

[![Image04](the-entity-framework-and-aspnet-getting-started-part-1/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image23.png)

## <a name="creating-the-entity-framework-data-model"></a>Creazione del modello di dati Entity Framework

A questo punto è possibile creare un modello di dati Entity Framework da questo database. È possibile creare il modello di dati nella cartella radice dell'applicazione, ma per questa esercitazione è possibile inserirlo in una cartella denominata *DAL* (per il livello di accesso ai dati).

Nelle **Esplora soluzioni**, aggiungere una cartella di progetto denominata *DAL* (assicurarsi che sia sotto il progetto, non incluse nella soluzione).

Fare doppio clic il *DAL* cartella e quindi selezionare **Add** e **nuovo elemento**. Sotto **modelli installati**, selezionare **Data**, selezionare il **ADO.NET Entity Data Model** modello, denominarla *SchoolModel*, e quindi fare clic su **Add**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-1/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image25.png)

Verrà avviata la procedura guidata Entity Data Model. Nel primo passaggio di procedura guidata, il **genera da database** opzione è selezionata per impostazione predefinita. Scegliere **Avanti**.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-1/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image27.png)

Nel **Seleziona connessione dati** passaggio, lasciare i valori predefiniti e fare clic su **successivo**. Il database School è selezionato per impostazione predefinita e l'impostazione di connessione viene salvata nel *Web. config* file come **SchoolEntities**.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-1/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image29.png)

Nel **Scegli oggetti di Database** passaggio della procedura guidata, selezionare tutte le tabelle tranne `sysdiagrams` (che è stato creato per il diagramma è stato generato in precedenza) e quindi fare clic su **fine**.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-1/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image31.png)

Dopo avere completato la creazione del modello, Visual Studio visualizza una rappresentazione grafica degli oggetti Entity Framework (entità) che corrispondono alle tabelle di database. (Come con il diagramma di database, la posizione dei singoli elementi potrebbe essere diversa da quello visualizzato in questa illustrazione. È possibile trascinare gli elementi circa in modo da corrispondere nell'illustrazione, se si desidera.)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-1/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image33.png)

## <a name="exploring-the-entity-framework-data-model"></a>Esplorazione del modello di dati di Entity Framework

È possibile vedere che il diagramma di entità è molto simile al diagramma di database, con un paio di differenze. Una differenza è l'aggiunta di simboli alla fine di ogni associazione che indicano il tipo di associazione (relazioni tra tabelle vengono chiamate associazioni di entità nel modello di dati):

- Un'associazione uno-a-zero-o-uno è rappresentata da "1" e "0..1".

    [![Image39](the-entity-framework-and-aspnet-getting-started-part-1/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image35.png)

    In questo caso, un `Person` entità può o non può essere associato un `OfficeAssignment` entità. Un' `OfficeAssignment` entità deve essere associata una `Person` entità. In altre parole, un docente può o non è possibile assegnare a un ufficio, e un ufficio può essere assegnato a solo un insegnante.
- Un'associazione uno-a-molti è rappresentata da "1" e "\*".

    [![Image40](the-entity-framework-and-aspnet-getting-started-part-1/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image37.png)

    In questo caso, un `Person` entità può o non possano essere associate `StudentGrade` entità. Oggetto `StudentGrade` deve essere associata a un'entità `Person` entità. `StudentGrade` le entità rappresentano effettivamente corsi registrati in questo database. Se uno studente è registrato in un corso e non è ancora disponibile, nessun livello di `Grade` proprietà è null. In altre parole, uno studente potrebbe non essere registrato in tutti i corsi ancora, è possibile registrare un corso oppure potrebbe essere iscritti a corsi più. Ciascun anno scolastico di un nuovo corso registrato si applica a un solo studente.
- Un'associazione molti-a-molti è rappresentata da "\*"e"\*".

    [![Image41](the-entity-framework-and-aspnet-getting-started-part-1/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image39.png)

    In questo caso, un `Person` entità può o non possano essere associate `Course` entità e viceversa è anche vero: una `Course` entità può o non possano essere associate `Person` entità. In altre parole, un docente può insegnare più corsi e un corso può essere impartito da più insegnanti. (In questo database, questa relazione si applica solo ai instructors (insegnanti); non è collegato studenti ai corsi. Gli studenti sono collegati ai corsi dalla tabella StudentGrades.)

Un'altra differenza tra il diagramma di database e il modello di dati è aggiuntiva **le proprietà di navigazione** sezione per ogni entità. Una proprietà di navigazione di un'entità fa riferimento a entità correlate. Ad esempio, il `Courses` proprietà in un `Person` entità contiene una raccolta di tutti il `Course` entità correlate a quella `Person` entità.

[![Image12](the-entity-framework-and-aspnet-getting-started-part-1/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image41.png)

Ancora un'altra differenza tra il modello di dati e del database è l'assenza del `CourseInstructor` tabella di associazione utilizzato nel database da collegare il `Person` e `Course` tabelle in una relazione molti-a-molti. Le proprietà di navigazione consentono di ottenere correlati `Course` entità dal `Person` entità ed elementi correlati `Person` entità dal `Course` entità, in modo che non è necessario per rappresentare la tabella di associazione nel modello di dati.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-1/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image43.png)

Ai fini di questa esercitazione, si supponga che il `FirstName` della colonna del `Person` tabella contiene effettivamente una persona primo nome e cognome. Si desidera modificare il nome del campo per riflettere questa situazione, ma l'amministratore del database (DBA) potrebbe non voler modificare il database. È possibile modificare il nome del `FirstName` proprietà nel modello di dati, lasciando il database equivalenti non modificato.

Nella finestra di progettazione, fare doppio clic su **FirstName** nel `Person` entità e quindi selezionare **rinominare**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-1/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image45.png)

Digitare il nome del nuovo "FirstMidName". Questa operazione modificherà il modo in cui che si fa riferimento alla colonna in codice senza modificare il database.

[![Image29](the-entity-framework-and-aspnet-getting-started-part-1/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image47.png)

Finestra browser modello offre un altro modo per visualizzare la struttura del database, la struttura del modello di dati e il mapping tra di essi. Per visualizzarlo, fare doppio clic su un'area vuota in entity designer e quindi fare clic su **Browser modello**.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-1/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image49.png)

Il **Browser modello** riquadro contiene una visualizzazione albero. (Il **Browser modello** riquadro può essere ancorato con la **Esplora soluzioni** riquadro.) Il **SchoolModel** nodo rappresenta la struttura del modello di data e il **SchoolModel. Store** nodo rappresenta la struttura del database.

[![Image26](the-entity-framework-and-aspnet-getting-started-part-1/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image51.png)

Espandere **SchoolModel. Store** per visualizzare le tabelle, espandere **tabelle / viste** per visualizzare le tabelle e quindi espandere **corso** per visualizzare le colonne all'interno di una tabella.

[![Image19](the-entity-framework-and-aspnet-getting-started-part-1/_static/image54.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image53.png)

Espandere **SchoolModel**, espandere **tipi di entità**, quindi espandere il **corso** nodo per visualizzare le entità e le proprietà dell'entità.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-1/_static/image56.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image55.png)

In una finestra di progettazione o la **Browser modello** riquadro è possibile visualizzare gli oggetti dei due modelli che intercorrono tra Entity Framework. Fare doppio clic il `Person` entità e selezionare **Mapping di tabelle**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-1/_static/image58.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image57.png)

Verrà visualizzata la **Dettagli Mapping** finestra. Si noti che questa finestra è possibile vedere che la colonna di database `FirstName` viene eseguito il mapping a `FirstMidName`, ossia ciò che è stata rinominata nel modello di dati.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-1/_static/image60.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image59.png)

Entity Framework utilizza XML per archiviare informazioni sul database, il modello di dati e i mapping tra di essi. Il *SchoolModel* file è in realtà un file XML che contiene queste informazioni. La finestra di progettazione esegue il rendering le informazioni contenute in un formato grafico, ma è anche possibile visualizzare il file XML facendo clic con il *edmx* del file in **Esplora soluzioni**, fare clic su **Apri con**e selezionando **Editor XML (testo)**. (La progettazione di modelli di data e un editor XML sono solo due diverse modalità di apertura e di utilizzo con lo stesso file, in modo da aprire e aprire il file in un editor XML nello stesso momento, non è possibile avere la finestra di progettazione).

È stato creato un sito Web, un database e un modello di dati. Nella procedura dettagliata successiva verrà iniziare a utilizzare i dati usando il modello di data e ASP.NET `EntityDataSource` controllo.

> [!div class="step-by-step"]
> [avanti](the-entity-framework-and-aspnet-getting-started-part-2.md)
