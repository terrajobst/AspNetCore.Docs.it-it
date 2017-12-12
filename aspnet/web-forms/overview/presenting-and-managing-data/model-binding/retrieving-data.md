---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Il recupero e visualizzazione dei dati con modello di associazione e web form | Documenti Microsoft
author: tfitzmac
description: "Questa serie di esercitazioni illustra gli aspetti di base dell'utilizzo di associazione del modello con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più semplice-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: e750250285fcb0088da284588d721ac21e73820c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>Il recupero e visualizzazione dei dati con l'associazione di modelli e web form
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questa serie di esercitazioni illustra gli aspetti di base dell'utilizzo di associazione del modello con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più semplice rispetto a gestione dati di oggetti di origine (ad esempio ObjectDataSource o SqlDataSource). Questa serie inizia con materiale introduttivo e sposta concetti più avanzati nelle esercitazioni successive.
> 
> Il modello di associazione del modello funziona con qualsiasi tecnologia di accesso ai dati. In questa esercitazione si utilizzerà Entity Framework, ma è possibile utilizzare la tecnologia di accesso ai dati che è più familiare. Da un controllo server associato a dati, ad esempio un controllo GridView, ListView, DetailsView o FormView, specificare i nomi dei metodi da utilizzare per la selezione, aggiornamento, eliminazione e creazione di dati. In questa esercitazione, si specificherà un valore per SelectMethod.
> 
> All'interno di tale metodo, fornire la logica per il recupero dei dati. Nella prossima esercitazione, si imposterà i valori per InsertMethod, UpdateMethod e DeleteMethod.
> 
> È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in c# o VB. Il codice scaricabile funziona con Visual Studio 2012 o Visual Studio 2013. Usa il modello di Visual Studio 2012, è leggermente diverso rispetto al modello di Visual Studio 2013 illustrato in questa esercitazione.
> 
> Nell'esercitazione si esegue l'applicazione in Visual Studio. È inoltre possibile rendere l'applicazione disponibile su Internet mediante la distribuzione di un provider di hosting. Microsoft offre l'hosting web gratuito per fino a 10 siti web in un  
>  [Azure account di valutazione gratuita](https://azure.microsoft.com/free/?WT.mc_id=A443DD604). Per informazioni su come distribuire un progetto web di Visual Studio per App Web di servizio App di Azure, vedere il [distribuzione Web ASP.NET utilizzando Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) serie. Inoltre, tale esercitazione viene illustrato come utilizzare le migrazioni di Entity Framework Code First per distribuire il database di SQL Server in Database SQL di Azure.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Microsoft Visual Studio 2013 o Microsoft Visual Studio Express 2013 per Web
>   
> 
> In questa esercitazione si integra inoltre con Visual Studio 2012, ma saranno alcune differenze nel modello di progetto e l'interfaccia utente.


## <a name="what-youll-build"></a>Occorre invece compilare

In questa esercitazione, effettuare le operazioni seguenti:

1. Creazione di oggetti dati che riflettono un'università con studenti iscritti a corsi
2. Compilare tabelle di database di oggetti
3. Popolare il database con dati di test
4. Visualizzare i dati in un web form

## <a name="set-up-project"></a>Configurazione di progetto

In Visual Studio 2013, creare un nuovo **applicazione Web ASP.NET** chiamato **ContosoUniversityModelBinding**.

![Crea progetto](retrieving-data/_static/image2.png)

Selezionare il modello Web Form e lasciare le altre opzioni predefinite. Fare clic su OK per il progetto di installazione.

![Selezionare i web form](retrieving-data/_static/image3.png)

Prima di tutto, verranno apportate alcune piccole modifiche per personalizzare l'aspetto del sito. Aprire il **Site. master** file e modificare il titolo in modo da includere University Contoso anziché l'applicazione ASP.NET.

[!code-aspx[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

Quindi, modificare il testo dell'intestazione da **nome applicazione** a **University Contoso**.

[!code-aspx[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

Anche in Site. master, è possibile modificare i collegamenti di navigazione che vengono visualizzati nell'intestazione in modo da riflettere le pagine rilevanti per questo sito. Non è necessario uno di **su** pagina o **contatto** pagina tali collegamenti possono essere rimossi. In alternativa, è necessario un collegamento a una pagina denominata **studenti**. Questa pagina non è ancora stata creata.

[!code-aspx[Main](retrieving-data/samples/sample3.aspx)]

Salvare e chiudere Site. master.

A questo punto, si creeranno il web form per visualizzare i dati di studenti. Mouse sul progetto, e **Aggiungi** un **nuovo elemento**. Selezionare il **Web Form con pagina Master** , modello e denominarlo **Students.aspx**.

![Creare una pagina](retrieving-data/_static/image5.png)

Selezionare **Site. master** come pagina master per il nuovo modulo web.

## <a name="create-the-data-models-and-database"></a>Creare i modelli di data e un database

Utilizzare migrazioni Code First per creare oggetti e le tabelle di database corrispondente. Queste tabelle archivia le informazioni riguardanti i corsi e studenti.

Nella cartella Models, aggiungere una nuova classe denominata **UniversityModels.cs**.

![creare una classe di modello](retrieving-data/_static/image7.png)

In questo file, definire le classi SchoolContext, studente, registrazione e corso come segue:

[!code-csharp[Main](retrieving-data/samples/sample4.cs)]

La classe SchoolContext deriva da DbContext, che gestisce la connessione al database e la modifica dei dati.

Nella classe Student, osservare gli attributi applicati per la **FirstName**, **LastName**, e **anno** proprietà. Questi attributi verranno utilizzati per la convalida dei dati in questa esercitazione. Per semplificare il codice per questo progetto di dimostrazione, queste proprietà sono contrassegnate con attributi di convalida dei dati. In un progetto reale, applicare gli attributi di convalida per tutte le proprietà che richiedono dati convalidati, ad esempio le proprietà nelle classi di registrazione e Course.

Salvare UniversityModels.cs.

Utilizzare gli strumenti per le migrazioni Code First per impostare un database in base a queste classi.

In **Package Manager Console**, eseguire il comando:  
`enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

Se il comando viene completato correttamente, si riceverà un messaggio indicante che sono state abilitate le migrazioni,

![abilitare le migrazioni](retrieving-data/_static/image8.png)

Si noti che un nuovo file denominato **Configuration.cs** è stato creato. In Visual Studio, questo file viene aperto automaticamente dopo la sua creazione. La classe di configurazione contiene un **valore di inizializzazione** metodo che consente di pre-popolare le tabelle di database con dati di test.

Aggiungere il codice seguente al metodo di inizializzazione. È necessario aggiungere un **utilizzando** istruzione per il **ContosoUniversityModelBinding.Models** dello spazio dei nomi.

[!code-csharp[Main](retrieving-data/samples/sample5.cs)]

Salvare Configuration.cs.

Nella Console di gestione pacchetti, eseguire il comando `add-migration initial`.

Quindi, eseguire il comando `update-database`.

Se si riceve un'eccezione quando si esegue questo comando, è possibile che i valori per StudentID e CourseID sono diverse dai valori nel metodo di inizializzazione. Aprire le tabelle nel database e trovare i valori esistenti per StudentID e CourseID. Nel codice per la tabella le registrazioni il seeding, aggiungere tali valori.

È stato aggiunto il file di database, ma al momento è nascosto nel progetto. Fare clic su **Mostra tutti i file** per visualizzare il file.

![Mostra tutti i file](retrieving-data/_static/image10.png)

Si noti il file con estensione mdf è ora visualizzato nell'App\_cartella dati.

![file di database](retrieving-data/_static/image12.png)

Fare doppio clic sul file con estensione mdf e aprire Esplora Server. Le tabelle ora esistano e vengono popolate con dati.

![tabelle di database](retrieving-data/_static/image14.png)

## <a name="display-data-from-students-and-related-tables"></a>Visualizzare i dati dalle tabelle correlate e gli studenti

Con i dati nel database, si è ora pronti per recuperare i dati e visualizzarli in una pagina web. Si utilizzerà un **GridView** controllo per visualizzare i dati in righe e colonne.

Aprire Students.aspx e individuare il **MainContent** segnaposto. All'interno di tale segnaposto, aggiungere un **GridView** controllo che include il codice seguente.

[!code-aspx[Main](retrieving-data/samples/sample6.aspx)]

Esistono un paio di concetti importanti in questo codice di markup in cui si noti. In primo luogo, si noti che è stato impostato un valore per il **SelectMethod** proprietà nell'elemento GridView. Questo valore specifica il nome del metodo utilizzato per il recupero dei dati per il controllo GridView. Questo metodo verrà creato nel passaggio successivo. In secondo luogo, si noti che il **ItemType** proprietà è impostata per la classe Student creato in precedenza. Impostando questo valore, è possibile fare riferimento alle proprietà di tale classe nel codice. Ad esempio, la classe Student contiene una raccolta denominata le registrazioni. È possibile utilizzare **Item.Enrollments** per recuperare tale insieme e quindi utilizzare la sintassi LINQ per recuperare la somma dei crediti registrati per ogni studente.

Nel file code-behind, è necessario aggiungere il metodo specificato per il **SelectMethod** valore. Aprire **Students.aspx.cs**e aggiungere **utilizzando** istruzioni per la **ContosoUniversityModelBinding.Models** e **System.Data.Entity**gli spazi dei nomi.

[!code-csharp[Main](retrieving-data/samples/sample7.cs)]

Quindi, aggiungere il metodo seguente. Si noti che il nome di questo metodo corrisponde al nome che immesso per SelectMethod.

[!code-csharp[Main](retrieving-data/samples/sample8.cs)]

Il **Include** clausola migliora le prestazioni della query ma non è essenziale per la query per il lavoro. Senza la clausola Include, è necessario recuperare i dati utilizzando il caricamento differito, che prevede l'invio di che una query separata per il database ogni volta correlati vengono recuperati i dati. Specificando la clausola Include, i dati vengono recuperati utilizzando il caricamento non differito, ovvero tutti i dati correlati viene recuperata mediante una singola query del database. Nei casi in cui la maggior parte dei dati correlati non è può essere meno efficiente caricamento eager, utilizzato in quanto vengono recuperati più dati. Tuttavia, in questo caso, caricamento eager fornisce prestazioni ottimali perché i dati correlati vengono visualizzati per ogni record.

Per ulteriori informazioni su considerazione sulle prestazioni durante il caricamento di dati correlati, vedere la sezione intitolata **Lazy Eager ed esplicita durante il caricamento di dati correlati** nel [lettura di dati correlati con Entity Framework in una pagina ASP Applicazione MVC .NET](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) argomento.

Per impostazione predefinita, i dati vengono ordinati i valori della proprietà contrassegnata come chiave. È possibile aggiungere una clausola OrderBy per specificare un valore diverso per l'ordinamento. In questo esempio, la proprietà StudentID predefinita viene utilizzata per l'ordinamento. Nel [ordinamento, Paging e filtro dei dati](sorting-paging-and-filtering-data.md) argomento, si consentirà all'utente di selezionare una colonna per l'ordinamento.

Eseguire l'applicazione web e passare alla pagina di studenti. La pagina di studenti Visualizza le informazioni seguenti di studenti.

![Mostra i dati](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>Generazione automatica di metodi di associazione di modelli

Quando questa serie di esercitazioni, è sufficiente copiare il codice dell'esercitazione al progetto. Tuttavia, uno svantaggio di questo approccio è che non possono che riconosca la funzionalità fornita da Visual Studio per generare automaticamente il codice per i metodi di associazione del modello. Quando si lavora su progetti, generazione automatica di codice può risparmiare tempo e la Guida è possibile ottenere un'idea di come implementare un'operazione. In questa sezione descrive la funzionalità di generazione automatica di codice. Questa sezione è solo informativo e non contiene il codice che è necessario implementare nel progetto.

Quando si imposta un valore per il **SelectMethod**, **UpdateMethod**, **InsertMethod**, o **DeleteMethod** proprietà nel codice di seguito, è possibile selezionare il **creare un nuovo metodo** opzione.

![creare nuovo metodo](retrieving-data/_static/image18.png)

Visual Studio non solo crea un metodo nel code-behind con la firma corretta, ma genera anche il codice di implementazione per facilitare l'esecuzione dell'operazione. Se si imposta innanzitutto il **ItemType** proprietà prima di utilizzare la funzionalità di generazione automatica di codice, il codice generato utilizzerà tale tipo per le operazioni. Quando si imposta la proprietà UpdateMethod, ad esempio, viene generato automaticamente il codice seguente:

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

Nuovamente, il codice precedente non è necessario essere aggiunti al progetto. Nella prossima esercitazione, verranno implementati i metodi per l'aggiornamento, eliminazione e aggiunta di nuovi dati.

## <a name="conclusion"></a>Conclusione

In questa esercitazione, si create classi di modello di dati e generata un database da tali classi. Le tabelle di database è stato compilato con dati di test. È utilizzata l'associazione del modello per recuperare i dati dal database e quindi visualizzati i dati in un controllo GridView.

Nella prossima [esercitazione](updating-deleting-and-creating-data.md) in questa serie, verrà abilitata l'aggiornamento, eliminazione e creazione di dati.

>[!div class="step-by-step"]
[Successivo](updating-deleting-and-creating-data.md)
