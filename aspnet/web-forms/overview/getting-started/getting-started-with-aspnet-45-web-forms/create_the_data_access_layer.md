---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
title: Creare il livello di accesso ai dati | Documenti Microsoft
author: Erikre
description: Questa serie di esercitazioni verranno fornite le nozioni di base della creazione di un'applicazione Web Form ASP.NET tramite ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per abbiamo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 0bbf7a6e-d7eb-4091-91e4-fff892777f32
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
msc.type: authoredcontent
ms.openlocfilehash: 809609155b06c4632bd4f450082d84c432c7a46f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="create-the-data-access-layer"></a>Creare il livello di accesso ai dati
====================
Da [Erik Reitan](https://github.com/Erikre)

[Scarica progetto di esempio Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [scaricare E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Questa serie di esercitazioni verranno fornite le nozioni di base della creazione di un'applicazione Web Form ASP.NET utilizzando ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per Web. Un Visual Studio 2013 [progetto con il codice sorgente c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) complemento questa serie di esercitazioni è disponibile.


In questa esercitazione viene descritto come creare, accedere ed esaminare i dati da un database utilizzando Code First di Entity Framework e Web Form ASP.NET. In questa esercitazione si basa sull'esercitazione precedente "Creare il progetto" e fa parte della serie di esercitazioni Wingtip giocattoli archivio. Dopo aver completato questa esercitazione, verrà creata un gruppo di classi di accesso ai dati presenti il *modelli* nella cartella del progetto.

## <a name="what-youll-learn"></a>Illustra quanto segue:

- Come creare i modelli di dati.
- Come inizializzare e inizializzazione del database.
- Come aggiornare e configurare l'applicazione per supportare il database.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Le funzionalità introdotte in questa esercitazione sono:

- Entity Framework Code First
- LocalDB
- Annotazioni dei dati

## <a name="creating-the-data-models"></a>Creazione dei modelli di dati

[Entity Framework](https://msdn.microsoft.com/data/aa937723) è un framework di mapping relazionale a oggetti (ORM). Consente di utilizzare dati relazionali come oggetti, eliminando la maggior parte del codice di accesso ai dati che è in genere necessario scrivere. Tramite Entity Framework, è possibile eseguire query utilizzando [LINQ](https://msdn.microsoft.com/library/bb397926.aspx), recuperare e manipolare i dati come oggetti fortemente tipizzati. LINQ fornisce modelli per l'esecuzione di query e l'aggiornamento dei dati. Utilizzo di Entity Framework consente di concentrarsi sulla creazione il resto dell'applicazione, piuttosto che sui dati nozioni di base di accesso. Più avanti in questa serie di esercitazioni, vi mostreremo come utilizzare i dati per popolare le query di navigazione e product.

Entity Framework supporta un paradigma di sviluppo denominato *Code First*. Code First consente di definire i modelli di dati utilizzando le classi. Una classe è un costrutto che consente di creare tipi personalizzati raggruppando le variabili di altri tipi, metodi ed eventi. È possibile eseguire il mapping delle classi a un database esistente o utilizzarle per generare un database. In questa esercitazione si creeranno i modelli di dati mediante la scrittura di classi del modello di dati. Quindi, si riceverà una Entity Framework creare il database in base a queste nuove classi.

Si inizierà creando le classi di entità che definiscono i modelli di dati per l'applicazione Web Form. Si creerà quindi una classe del contesto che gestisce le classi di entità e fornisce accesso ai dati nel database. Si creerà inoltre una classe di inizializzatore che verrà utilizzato per popolare il database.

### <a name="entity-framework-and-references"></a>I riferimenti e entity Framework

Per impostazione predefinita, Entity Framework è incluso quando si crea un nuovo **applicazione Web ASP.NET** utilizzando il **Web Form** modello. Entity Framework è possibile installare, disinstallare e aggiornare come pacchetto NuGet.

Questo pacchetto NuGet include quanto segue **runtime** assembly all'interno del progetto:

- File EntityFramework. dll: tutto il codice di runtime comune utilizzato da Entity Framework
- EntityFramework.SqlServer.dll: il provider di Microsoft SQL Server per Entity Framework

### <a name="entity-classes"></a>Classi di entità

Le classi create per definire lo schema dei dati sono dette classi di entità. Se si ha familiarità con Progettazione database, considerare le classi di entità come definizioni di tabella di un database. Ogni proprietà nella classe specifica una colonna nella tabella del database. Queste classi forniscono un'interfaccia leggera, relazionale a oggetti tra codice orientata agli oggetti e la struttura della tabella relazionale del database.

In questa esercitazione, verrà inizialmente tramite l'aggiunta di classi di entità semplici che rappresentano gli schemi per i prodotti e le categorie. La classe di prodotti conterrà le definizioni per ciascun prodotto. Il nome di ogni membro della classe product sarà `ProductID`, `ProductName`, `Description`, `ImagePath`, `UnitPrice`, `CategoryID`, e `Category`. La classe della categoria conterrà le definizioni per ogni categoria di un prodotto può appartenere a, ad esempio Auto, barca o piano. Il nome di ogni membro della classe di categorie sarà `CategoryID`, `CategoryName`, `Description`, e `Products`. Ogni prodotto appartiene a una delle categorie. Queste classi di entità verranno aggiunto il progetto esistente *modelli* cartella.

1. In **Esplora**, fare doppio clic su di *modelli* cartella e quindi selezionare **Aggiungi**  - &gt; **nuovo elemento**. 

    ![Creare il livello di accesso ai dati - nuovo elemento Menu](create_the_data_access_layer/_static/image1.png)

 Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. In **Visual c#** dal **installato** riquadro a sinistra, seleziona **codice**. 

    ![Creare il livello di accesso ai dati - nuovo elemento Menu](create_the_data_access_layer/_static/image2.png)
3. Selezionare **classe** dal riquadro centrale e denominare la nuova classe *Product.cs*.
4. Fare clic su **Aggiungi**.  
 Il nuovo file di classe viene visualizzato nell'editor.
5. Sostituire il codice predefinito con il codice seguente:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample1.cs)]
6. Creare un'altra classe ripetendo i passaggi da 1 a 4, nome, tuttavia, la nuova classe *Category.cs* e sostituire il codice predefinito con il codice seguente:  

    [!code-csharp[Main](create_the_data_access_layer/samples/sample2.cs)]

Come menzionato in precedenza, il `Category` classe rappresenta il tipo di prodotto che l'applicazione è progettata per vendere (ad esempio <a id="a"> </a> &quot;automobili&quot;, &quot;barche&quot;, &quot;Razzi&quot;e così via) e `Product` classe rappresenta i singoli prodotti (toys) nel database. Ogni istanza di un `Product` oggetto corrisponderà a una riga all'interno di una tabella di database relazionali e ciascuna proprietà della classe Product verrà eseguito il mapping a una colonna nella tabella di database relazionale. Più avanti in questa esercitazione, si saranno esaminare i dati di prodotto contenuti nel database.

### <a name="data-annotations"></a>Annotazioni dei dati

Si è notato che determinati membri delle classi hanno attributi specificando dettagli sul membro, ad esempio `[ScaffoldColumn(false)]`. Si tratta di *le annotazioni dei dati*. Gli attributi di annotazione dei dati è possono descrivere come convalidare l'input dell'utente per il membro, per specificare la formattazione per tale e per specificare come si è modellato quando viene creato il database.

### <a name="context-class"></a>Classe Context

Per iniziare a utilizzare le classi per accedere ai dati, è necessario definire una classe del contesto. Come accennato in precedenza, la classe del contesto gestisce le classi di entità (ad esempio il `Product` classe e `Category` classe) e fornisce l'accesso ai dati al database.

Questa procedura consente di aggiungere una nuovo contesto classe c# per il *modelli* cartella.

1. Fare doppio clic su di *modelli* cartella e quindi selezionare **Aggiungi**  - &gt; **nuovo elemento**.   
 Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Selezionare **classe** dal riquadro centrale, denominarla *ProductContext.cs* e fare clic su **Aggiungi**.
3. Sostituire il codice predefinito contenuto nella classe con il codice seguente:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample3.cs)]

Questo codice aggiunge il `System.Data.Entity` dello spazio dei nomi in modo da poter accedere a tutte le funzionalità principali di Entity Framework, che include la possibilità di query, inserire, aggiornare ed eliminare dati quando si lavora con oggetti fortemente tipizzati.

Il `ProductContext` classe rappresenta il contesto del database di Entity Framework prodotto, che gestisce il recupero, l'archiviazione e l'aggiornamento `Product` classe istanze nel database. Il `ProductContext` deriva dalla classe di `DbContext` classe forniti da Entity Framework di base.

### <a name="initializer-class"></a>Classe di inizializzatore

È necessario eseguire una logica personalizzata per inizializzare l'ora del primo database che viene utilizzato il contesto. In questo modo i dati iniziali da aggiungere al database in modo che è possibile visualizzare immediatamente i prodotti e le categorie.

Questa procedura consente di aggiungere una nuovo inizializzatore classe c# per il *modelli* cartella.

1. Creare un altro `Class` nel *modelli* cartella e denominarla *ProductDatabaseInitializer.cs*.
2. Sostituire il codice predefinito contenuto nella classe con il codice seguente:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample4.cs)]

Come si può notare nel codice precedente, quando il database viene creato e inizializzato, il `Seed` proprietà è sottoposto a override e impostata. Quando il `Seed` è impostata, i valori dalle categorie e i prodotti vengono utilizzati per popolare il database. Se si tenta di aggiornare i dati di inizializzazione modificando il codice precedente dopo aver creato il database, non verranno visualizzati tutti gli aggiornamenti quando si esegue l'applicazione Web. Il motivo è il codice precedente utilizza un'implementazione del `DropCreateDatabaseIfModelChanges` classe riconoscere se il modello (schema) è stato modificato prima di ripristinare i dati del valore di inizializzazione. Se non vengono apportate modifiche per il `Category` e `Product` classi di entità, il database non verranno reinizializzate con i dati del valore di inizializzazione.

> [!NOTE] 
> 
> Se si desidera che il database viene ricreato ogni volta che è stata eseguita l'applicazione, è possibile utilizzare il `DropCreateDatabaseAlways` classe anziché la `DropCreateDatabaseIfModelChanges` classe. Tuttavia per questa serie di esercitazioni, usare la `DropCreateDatabaseIfModelChanges` classe.


A questo punto in questa esercitazione, si avrà un *modelli* cartella con quattro nuove classi e la classe di un valore predefinito:

![Creare a livello di accesso ai dati - cartella modelli](create_the_data_access_layer/_static/image3.png)

### <a name="configuring-the-application-to-use-the-data-model"></a>Configurazione dell'applicazione per utilizzare il modello di dati

Dopo aver creato le classi che rappresentano i dati, è necessario configurare l'applicazione per utilizzare le classi. Nel *Global. asax* file, Aggiungi il codice che inizializza il modello. Nel *Web. config* file aggiunto le informazioni che indicano l'applicazione, quali database che si userà per archiviare i dati che sono rappresentati da nuove classi di dati. Il *Global. asax* file può essere utilizzato per gestire gli eventi dell'applicazione o i metodi. Il *Web. config* file consente di controllare la configurazione dell'applicazione web ASP.NET.

#### <a name="updating-the-globalasax-file"></a>L'aggiornamento del file Global. asax

Per inizializzare i modelli di data all'avvio dell'applicazione, si aggiornerà il `Application_Start` gestore il *Global.asax.cs* file.

> [!NOTE] 
> 
> In Esplora soluzioni, è possibile selezionare il *Global. asax* file o *Global.asax.cs* file per la modifica di *Global.asax.cs* file.


1. Aggiungere il seguente codice evidenziato in giallo per il `Application_Start` metodo il *Global.asax.cs* file.   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample5.cs?highlight=9-10,22-23)]

> [!NOTE] 
> 
> Il browser supporti HTML5 per visualizzare il codice evidenziato in giallo durante la visualizzazione di questa serie di esercitazioni in un browser.


Come illustrato nel codice precedente, all'avvio dell'applicazione, l'applicazione specifica l'inizializzatore che verrà eseguiti durante la prima volta che i dati si accede. I due spazi dei nomi aggiuntivi sono necessari per l'accesso di `Database` oggetto e `ProductDatabaseInitializer` oggetto.

 Modifica del File Web. config 

Anche se Entity Framework Code First verrà generato un database di in un percorso predefinito quando il database viene popolato con i dati di inizializzazione, l'aggiunta di informazioni di connessione per l'applicazione consente di controllo del percorso del database. Specificare la connessione al database utilizzando una stringa di connessione dell'applicazione *Web. config* file alla radice del progetto. Aggiungendo una nuova stringa di connessione, è possibile indirizzare il percorso del database (*wingtiptoys.mdf*) per essere compilato nella directory dei dati dell'applicazione (*App\_dati*), anziché sul valore predefinito percorso. Questa modifica consente di individuare ed esaminare il file di database più avanti in questa esercitazione.

1. In **Esplora**, trovare e aprire il *Web. config* file.
2. Aggiungere la stringa di connessione evidenziata in giallo per il `<connectionStrings>` sezione la *Web. config* file come segue:  

    [!code-xml[Main](create_the_data_access_layer/samples/sample6.xml?highlight=4-7)]

Quando l'applicazione viene eseguita per la prima volta, creerà il database nel percorso specificato dalla stringa di connessione. Ma prima di eseguire l'applicazione, creare ora, prima di tutto.

## <a name="building-the-application"></a>Compilazione dell'applicazione

Per assicurarsi che tutte le classi e le modifiche all'applicazione Web di lavoro, è necessario compilare l'applicazione.

1. Dal **Debug** dal menu **compilare WingtipToys**.  
 Il **Output** verrà visualizzata la finestra e se l'assenza di errori, viene visualizzato un *ha avuto esito positivo* messaggio.  

    ![Creare il livello di accesso ai dati - finestra di Output](create_the_data_access_layer/_static/image4.png)

In caso di errore, ricontrollare i passaggi precedenti. Le informazioni contenute nel **Output** finestra indicherà quali file ha un problema e in cui il file è necessaria una modifica. Queste informazioni consentono di determinare quale parte della procedura devono essere esaminato e risolto nel progetto.

## <a name="summary"></a>Riepilogo

In questa esercitazione della serie è avere creato il modello di dati, nonché, aggiunto il codice che verrà utilizzato per inizializzare e inizializzazione del database. L'applicazione di utilizzare i modelli di data quando l'applicazione viene eseguita anche è stata configurata.

Nella prossima esercitazione, si sarà aggiornare l'interfaccia utente, aggiungere la navigazione e recuperare dati dal database. Verrà creato il database viene creato automaticamente in base alle classi di entità che è stato creato in questa esercitazione.

## <a name="additional-resources"></a>Risorse aggiuntive

[Panoramica di Entity Framework](https://msdn.microsoft.com/library/bb399567.aspx)   
[Guida introduttiva a ADO.NET Entity Framework](https://msdn.microsoft.com/data/ee712907)   
[Prima di sviluppo con Entity Framework di codice](http://www.msteched.com/2010/Europe/DEV212) (video)   
[Primo relazioni Fluent API del codice](https://msdn.microsoft.com/data/hh134698)   
[Codice prima le annotazioni dei dati](https://msdn.microsoft.com/data/gg193958)  
[Miglioramenti di produttività per Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)

>[!div class="step-by-step"]
[Precedente](create-the-project.md)
[Successivo](ui_and_navigation.md)
