---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Il recupero e visualizzazione dei dati con modello di associazione e web form | Microsoft Docs
author: tfitzmac
description: Questa serie di esercitazioni illustra aspetti di base dell'uso di associazione di modelli con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più linee rette-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: 4a76fcfd2620eb0789b7a6a23990037a1eebaf4c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378495"
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>Il recupero e visualizzazione dei dati con l'associazione di modelli e web form
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> Questa serie di esercitazioni illustra aspetti di base dell'uso di associazione di modelli con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più semplice rispetto a gestione dati di oggetti di origine (ad esempio ObjectDataSource o SqlDataSource). Questa serie inizia con materiale introduttivo e sposta i concetti più avanzati nelle esercitazioni successive.
> 
> Il modello di associazione del modello funziona con qualsiasi tecnologia di accesso ai dati. In questa esercitazione si userà Entity Framework, ma è possibile usare la tecnologia di accesso ai dati che è più nota all'utente. Da un controllo server associato a dati, ad esempio un controllo GridView, ListView, DetailsView e FormView, si specificano i nomi dei metodi da usare per la selezione, l'aggiornamento, eliminazione e creazione di dati. In questa esercitazione, si specificherà un valore per la proprietà SelectMethod.
> 
> All'interno di tale metodo, per fornire la logica per recuperare i dati. Nella prossima esercitazione, si imposteranno i valori per UpdateMethod e InsertMethod e DeleteMethod.
> 
> È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in c# o VB. Il codice scaricabile funziona con Visual Studio 2012 o Visual Studio 2013. Usa il modello di Visual Studio 2012, che è leggermente diverso rispetto al modello di Visual Studio 2013 illustrato in questa esercitazione.
> 
> In questa esercitazione si esegue l'applicazione in Visual Studio. Si può anche rendere disponibile l'applicazione su Internet tramite distribuzione su un provider di hosting. Microsoft offre l'hosting web gratuito per fino a 10 siti web in un  
>  [account Azure gratuito](https://azure.microsoft.com/free/?WT.mc_id=A443DD604). Per informazioni su come distribuire un progetto web Visual Studio per App Web di servizio App di Azure, vedere la [distribuzione Web ASP.NET tramite Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) serie. Tale esercitazione illustra anche come usare le migrazioni di Entity Framework Code First per distribuire il database di SQL Server in Database SQL di Azure.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Microsoft Visual Studio 2013 o Microsoft Visual Studio Express 2013 per Web
>   
> 
> Questa esercitazione funziona anche con Visual Studio 2012, ma vi sono alcune differenze nel modello di progetto e dell'interfaccia utente.


## <a name="what-youll-build"></a>Scopo dell'esercitazione

In questa esercitazione, sarà:

1. Creare oggetti dati che riflettono un'università con studenti iscritti a corsi
2. Creare tabelle di database dagli oggetti
3. Popolare il database con dati di test
4. Visualizzare i dati in un web form

## <a name="set-up-project"></a>Configurazione di progetto

In Visual Studio 2013, creare una nuova **applicazione Web ASP.NET** chiamato **ContosoUniversityModelBinding**.

![Crea progetto](retrieving-data/_static/image2.png)

Selezionare il modello Web Form e lasciare le altre opzioni predefinite. Fare clic su OK per il progetto di installazione.

![Selezionare web form](retrieving-data/_static/image3.png)

In primo luogo, si apporterà alcune piccole modifiche per personalizzare l'aspetto del sito. Aprire il **Site. master** file e modificare il titolo in modo da includere Contoso University anziché My ASP.NET Application.

[!code-aspx[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

Quindi, modificare il testo dell'intestazione dal **nome dell'applicazione** al **Contoso University**.

[!code-aspx[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

Anche in Site. master, modificare i collegamenti di navigazione che vengono visualizzati nell'intestazione in modo da riflettere le pagine che sono rilevanti per questo sito. Non è necessario essere il **sulle** pagina o il **contatto** pagina in modo che tali collegamenti possono essere rimossi. In alternativa, è necessario un collegamento a una pagina denominata **studenti**. Questa pagina non è ancora stata creata.

[!code-aspx[Main](retrieving-data/samples/sample3.aspx)]

Salvare e chiudere Site. master.

A questo punto, si creerà il web form per visualizzare i dati degli studenti. Fare clic sul progetto, e **Add** una **nuovo elemento**. Selezionare il **Web Form con pagina Master** modello e denominarla **Students.aspx**.

![pagina Crea](retrieving-data/_static/image5.png)

Selezionare **Site. master** come pagina master per il nuovo modulo web.

## <a name="create-the-data-models-and-database"></a>Creare il database e i modelli di dati

Si userà migrazioni Code First per creare oggetti e tabelle del database corrispondente. Queste tabelle archivia informazioni riguardanti gli studenti e i rispettivi corsi.

Nella cartella Models, aggiungere una nuova classe denominata **UniversityModels.cs**.

![creare una classe di modello](retrieving-data/_static/image7.png)

In questo file, definire le classi SchoolContext, studente, registrazione e corsi come indicato di seguito:

[!code-csharp[Main](retrieving-data/samples/sample4.cs)]

La classe SchoolContext deriva da DbContext, che gestisce la connessione al database e le modifiche nei dati.

Nella classe Student, si noti che gli attributi che sono stati applicati per il **FirstName**, **LastName**, e **anno** proprietà. Questi attributi verranno utilizzati per la convalida dei dati in questa esercitazione. Per semplificare il codice per il progetto dimostrativo, queste proprietà sono contrassegnate con gli attributi di convalida dei dati. In un progetto reale, gli attributi di convalida viene applicato a tutte le proprietà che richiedono dati convalidati, ad esempio le proprietà nelle classi di registrazione e lo stesso corso.

Salvare UniversityModels.cs.

Si userà gli strumenti per migrazioni Code First per configurare un database in base a queste classi.

Nelle **Console di gestione pacchetti**, eseguire il comando:  
`enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

Se il comando viene completato correttamente si riceverà un messaggio che indica che le migrazioni sono state abilitate,

![abilitare migrazioni](retrieving-data/_static/image8.png)

Si noti che un nuovo file denominato **Configuration.cs** è stato creato. In Visual Studio, questo file viene aperto automaticamente dopo averlo creato. La classe di configurazione contiene un **Seed** metodo che consente di pre-popolare le tabelle di database con dati di test.

Aggiungere il codice seguente al metodo di inizializzazione. È necessario aggiungere un **usando** istruzione per il **ContosoUniversityModelBinding.Models** dello spazio dei nomi.

[!code-csharp[Main](retrieving-data/samples/sample5.cs)]

Salvare Configuration.cs.

Nella Console di gestione pacchetti eseguire il comando `add-migration initial`.

Quindi, eseguire il comando `update-database`.

Se si riceve un'eccezione quando si esegue questo comando, è possibile che i valori per CourseID e StudentID sono diversi dai valori il metodo di inizializzazione. Aprire le tabelle nel database e trovare i valori esistenti per CourseID e StudentID. Nel codice per il seeding della tabella di registrazioni, aggiungere tali valori.

È stato aggiunto il file di database, ma è attualmente nascosto nel progetto. Fare clic su **Mostra tutti i file** per visualizzare il file.

![Mostra tutti i file](retrieving-data/_static/image10.png)

Si noti che il file. mdf viene ora visualizzato nell'App\_cartella dati.

![file di database](retrieving-data/_static/image12.png)

Fare doppio clic sul file con estensione mdf e aprire Esplora Server. Le tabelle ora esistano e vengono popolate con dati.

![tabelle di database](retrieving-data/_static/image14.png)

## <a name="display-data-from-students-and-related-tables"></a>Visualizzare i dati dalle tabelle correlate e gli studenti

Con i dati nel database, si è ora pronti per recuperare i dati e visualizzarli in una pagina web. Si userà una **GridView** controllo per visualizzare i dati in colonne e righe.

Aprire Students.aspx e individuare il **MainContent** segnaposto. All'interno di tale segnaposto, aggiungere un **GridView** controllo che include il codice seguente.

[!code-aspx[Main](retrieving-data/samples/sample6.aspx)]

Esistono un paio di concetti importanti in questo codice di markup per sottolineare. In primo luogo, si noti che viene impostato un valore per il **SelectMethod** proprietà nell'elemento GridView. Questo valore specifica il nome del metodo che viene usato per il recupero dei dati per il controllo GridView. Questo metodo verrà creato nel passaggio successivo. In secondo luogo, si noti che il **ItemType** viene impostata per la classe Student creata in precedenza. Impostando questo valore, è possibile fare riferimento alle proprietà di tale classe nel codice di markup. Ad esempio, la classe Student contiene una raccolta denominata le registrazioni. È possibile usare **Item.Enrollments** per recuperare tale insieme e quindi usare la sintassi LINQ per recuperare la somma dei crediti registrati per ogni studente.

Nel file code-behind, è necessario aggiungere il metodo specificato per il **SelectMethod** valore. Aprire **Students.aspx.cs**e aggiungere **usando** istruzioni per il **ContosoUniversityModelBinding.Models** e **Data. Entity**spazi dei nomi.

[!code-csharp[Main](retrieving-data/samples/sample7.cs)]

Aggiungere quindi il metodo seguente. Si noti che il nome di questo metodo corrisponde al nome specificato per la proprietà SelectMethod.

[!code-csharp[Main](retrieving-data/samples/sample8.cs)]

Il **inclusione** clausola migliora le prestazioni delle query ma non è essenziale per la query da usare. Senza la clausola Include, i dati potrebbero essere recuperati usando il caricamento lazy, che prevede l'invio di che una query separata per il database ogni volta che correlati vengono recuperati i dati. Fornendo la clausola Include, i dati vengono recuperati tramite il caricamento eager, ovvero tutti i dati correlati vengono recuperati tramite una singola query del database. Nei casi in cui la maggior parte dei dati correlati non essere il caricamento eager, utilizzato può rivelarsi meno efficiente perché vengono recuperati più dati. Tuttavia, in questo caso, il caricamento eager fornisce prestazioni ottimali perché i dati correlati vengono visualizzati per ogni record.

Per altre informazioni sulle considerazioni relative alle prestazioni quando si caricano i dati correlati, vedere la sezione intitolata **esplicito il caricamento di dati correlati Lazy ed Eager** nel [la lettura di dati correlati con Entity Framework in un piano ASP Applicazione MVC .NET](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) argomento.

Per impostazione predefinita, i dati vengono ordinati i valori della proprietà contrassegnata come chiave. È possibile aggiungere una clausola OrderBy per specificare un valore diverso per l'ordinamento. In questo esempio, la proprietà StudentID predefinita viene utilizzata per l'ordinamento. Nel [ordinamento, Paging e filtro dei dati](sorting-paging-and-filtering-data.md) argomento, si consentirà all'utente di selezionare una colonna per l'ordinamento.

Eseguire l'applicazione web e passare alla pagina Students. La pagina Students Visualizza le informazioni seguenti per studenti.

![visualizzare i dati](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>Generazione automatica di metodi di associazione di modelli

Quando questa serie di esercitazioni, è possibile copiare semplicemente il codice dell'esercitazione al progetto. Tuttavia, uno svantaggio di questo approccio è che si potrebbe non viene a conoscenza della funzionalità fornite da Visual Studio per generare automaticamente codice per metodi di associazione di modelli. Quando si lavora su progetti, generazione automatica di codice può risparmiare tempo e la Guida è possibile ottenere un'idea di come implementare un'operazione. In questa sezione viene descritta la funzionalità di generazione automatica di codice. Questa sezione è solo informativo e non contiene alcun codice che è necessario implementare nel progetto.

Quando si imposta un valore per il **SelectMethod**, **UpdateMethod**, **InsertMethod**, oppure **DeleteMethod** proprietà nel codice, markup è possibile selezionare i **Crea nuovo metodo** opzione.

![creare nuovo metodo](retrieving-data/_static/image18.png)

Visual Studio non solo crea un metodo nel code-behind con la firma appropriata, ma genera anche codice di implementazione per agevolare l'esecuzione dell'operazione. Se si imposta innanzitutto le **ItemType** proprietà prima di usare la funzionalità di generazione automatica di codice, il codice generato utilizzerà tale tipo per le operazioni. Ad esempio, quando si imposta la proprietà UpdateMethod, viene generato automaticamente il codice seguente:

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

Anche in questo caso, il codice sopra riportato non dovrà essere aggiunto al progetto. Nella prossima esercitazione, si implementerà i metodi per l'aggiornamento, eliminazione e aggiunta di nuovi dati.

## <a name="conclusion"></a>Conclusione

In questa esercitazione si create classi di modello di dati e generata un database da tali classi. Le tabelle del database è stato compilato con i dati di test. È utilizzata l'associazione di modelli per recuperare dati dal database e quindi visualizzati i dati in un controllo GridView.

Entro i prossimi [esercitazione](updating-deleting-and-creating-data.md) in questa serie si abiliterà l'aggiornamento, eliminazione e creazione di dati.

> [!div class="step-by-step"]
> [avanti](updating-deleting-and-creating-data.md)
