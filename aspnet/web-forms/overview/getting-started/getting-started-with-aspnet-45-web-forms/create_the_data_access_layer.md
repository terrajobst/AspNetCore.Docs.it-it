---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
title: Creare il livello di accesso ai dati | Microsoft Docs
author: Erikre
description: Questa serie di esercitazioni insegnerà le nozioni di base della creazione di un'applicazione Web Form ASP.NET con ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per Microsoft...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 0bbf7a6e-d7eb-4091-91e4-fff892777f32
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
msc.type: authoredcontent
ms.openlocfilehash: 5b4bb5bc89938836bc37e8ebd385fa966bc6f511
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390033"
---
<a name="create-the-data-access-layer"></a>Creare il livello di accesso ai dati
====================
da [Erik Reitan](https://github.com/Erikre)

[Scaricare progetto di esempio Wingtip Toys (c#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [Scarica l'E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Questa serie di esercitazioni insegnerà le nozioni di base della creazione di un'applicazione Web Form ASP.NET con ASP.NET 4.5 e Microsoft Visual Studio Express 2013 per Web. Un Visual Studio 2013 [progetto con codice sorgente c#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) complemento a questa serie di esercitazioni è disponibile.


Questa esercitazione descrive come creare, accedere ed esaminare i dati da un database tramite Entity Framework Code First e Web Form ASP.NET. Questa esercitazione si basa sull'esercitazione precedente "Creare il progetto" e fa parte della serie di esercitazioni di Wingtip giocattoli Store. Dopo aver completato questa esercitazione, verrà generata un gruppo di classi di accesso ai dati inclusi tra i *modelli* cartella del progetto.

## <a name="what-youll-learn"></a>Che cosa si apprenderà come:

- Come creare i modelli di dati.
- Come inizializzare ed eseguire il seeding del database.
- Come aggiornare e configurare l'applicazione per supportare il database.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Queste sono le funzionalità introdotte in questa esercitazione:

- Entity Framework Code First
- LocalDB
- Annotazioni dei dati

## <a name="creating-the-data-models"></a>Creazione di modelli di Data

[Entity Framework](https://msdn.microsoft.com/data/aa937723) è un framework object relational mapping (ORM). Consente di utilizzare dati relazionali come oggetti, eliminando la maggior parte del codice di accesso ai dati che è in genere necessario scrivere. Usa Entity Framework, è possibile eseguire query che usano [LINQ](https://msdn.microsoft.com/library/bb397926.aspx), quindi recuperare e manipolare i dati come oggetti fortemente tipizzati. LINQ fornisce modelli per l'esecuzione di query e aggiornamento dei dati. Usa Entity Framework consente di concentrarsi sulla creazione di altri elementi dell'applicazione, piuttosto che sui dati nozioni fondamentali di accesso. Più avanti in questa serie di esercitazioni, mostreremo come usare i dati per popolare le query di navigazione e product.

Entity Framework supporta un paradigma di sviluppo denominato *Code First*. Code First consente di definire modelli di dati con le classi. Una classe è un costrutto che consente di creare tipi personalizzati raggruppando insieme variabili di altri tipi, metodi ed eventi. È possibile eseguire il mapping di classi a un database esistente o usarle per generare un database. In questa esercitazione si creerà i modelli di dati mediante la scrittura di classi di modello di dati. Quindi, si riceverà una Entity Framework crea il database in tempo reale da queste nuove classi.

Inizierà creando le classi di entità che definiscono i modelli di dati per l'applicazione Web Form. Quindi si creerà una classe del contesto che gestisce le classi di entità e fornisce accesso ai dati nel database. Si creerà inoltre una classe di inizializzatore che si userà per popolare il database.

### <a name="entity-framework-and-references"></a>Riferimenti ed entity Framework

Per impostazione predefinita, Entity Framework è incluso quando si crea una nuova **applicazione Web ASP.NET** usando la **Web Form** modello. Entity Framework può essere installata, disinstallata e aggiornato come pacchetto NuGet.

Questo pacchetto NuGet include quanto segue **runtime** assembly all'interno del progetto:

- EntityFramework: tutto il codice di runtime comuni usato da Entity Framework
- EntityFramework.SqlServer.dll: il provider di Microsoft SQL Server per Entity Framework

### <a name="entity-classes"></a>Classi di entità

Le classi create per definire lo schema dei dati vengono definite le classi di entità. Se si ha familiarità con progettazione di database, considerare le classi di entità come definizioni di tabella di un database. Ogni proprietà nella classe specifica una colonna nella tabella del database. Queste classi forniscono un'interfaccia leggera, relazionale a oggetti tra il codice basato sugli oggetti e la struttura della tabella relazionale del database.

In questa esercitazione, sarà inizialmente aggiungendo le classi di entità semplice che rappresenta gli schemi per i prodotti e le categorie. La classe di prodotti conterrà le definizioni per ciascun prodotto. Il nome di ogni membro della classe product saranno `ProductID`, `ProductName`, `Description`, `ImagePath`, `UnitPrice`, `CategoryID`, e `Category`. La classe di categorie conterrà le definizioni per ogni categoria che un prodotto può appartenere a, ad esempio automobili, barca o piano. Il nome di ogni membro della classe categoria saranno `CategoryID`, `CategoryName`, `Description`, e `Products`. Ogni prodotto appartiene a una delle categorie. Queste classi di entità verranno aggiunti a quello del progetto esistente *modelli* cartella.

1. Nella **Esplora soluzioni**, fare doppio clic il *modelli* cartella e quindi selezionare **Add**  - &gt; **nuovo elemento**. 

    ![Creare il livello di accesso ai dati - Menu nuovo elemento](create_the_data_access_layer/_static/image1.png)

   Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Sotto **Visual c#** dalle **installati** riquadro a sinistra, seleziona **codice**. 

    ![Creare il livello di accesso ai dati - Menu nuovo elemento](create_the_data_access_layer/_static/image2.png)
3. Selezionare **classe** dal riquadro centrale e nuova classe il nome *Product.cs*.
4. Fare clic su **Aggiungi**.  
   Il nuovo file di classe viene visualizzato nell'editor.
5. Sostituire il codice predefinito con il codice seguente:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample1.cs)]
6. Creare un'altra classe ripetendo i passaggi da 1 a 4, tuttavia, assegnare un nome alla nuova classe *Category.cs* e sostituire il codice predefinito con il codice seguente:  

    [!code-csharp[Main](create_the_data_access_layer/samples/sample2.cs)]

Come accennato in precedenza, il `Category` classe rappresenta il tipo di prodotto che l'applicazione è progettata per la vendita (ad esempio <a id="a"> </a> &quot;automobili&quot;, &quot;evolveranno&quot;, &quot;Razzi&quot;e così via) e il `Product` classe rappresenta i singoli prodotti (toys) nel database. Ogni istanza di un `Product` oggetto corrisponderà a una riga all'interno di una tabella di database relazionale e ogni proprietà della classe Product verrà eseguito il mapping a una colonna nella tabella di database relazionale. Più avanti in questa esercitazione, si esaminati i dati del prodotto contenuti nel database.

### <a name="data-annotations"></a>Annotazioni dei dati

Si è notato che alcuni membri delle classi dispongono di attributi specificando dettagli sul membro, ad esempio `[ScaffoldColumn(false)]`. Si tratta *annotazioni dei dati*. Gli attributi di annotazione dei dati possono descrivere come convalidare l'input dell'utente per il membro, per specificare la formattazione per tale e per specificare come si è modellato quando viene creato il database.

### <a name="context-class"></a>Classe Context

Per iniziare a usare le classi per l'accesso ai dati, è necessario definire una classe del contesto. Come indicato in precedenza, la classe del contesto gestisce le classi di entità (ad esempio la `Product` classe e il `Category` classe) e fornisce accesso ai dati nel database.

Questa procedura consente di aggiungere una nuovo contesto classe c# per il *modelli* cartella.

1. Fare doppio clic il *modelli* cartella e quindi selezionare **Add**  - &gt; **nuovo elemento**.   
   Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Selezionare **classe** dal riquadro centrale, denominarla *ProductContext.cs* e fare clic su **Add**.
3. Sostituire il codice predefinito contenuto nella classe con il codice seguente:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample3.cs)]

Questo codice aggiunge il `System.Data.Entity` dello spazio dei nomi in modo da poter accedere a tutte le funzionalità core di Entity Framework, che include la possibilità di query, inserire, aggiornare ed eliminare dati utilizzando oggetti fortemente tipizzati.

Il `ProductContext` classe rappresenta il contesto del database di Entity Framework del prodotto, che gestisce il recupero, l'archiviazione e aggiornando `Product` classe istanze nel database. Il `ProductContext` deriva dalla classe di `DbContext` classe fornita da Entity Framework di base.

### <a name="initializer-class"></a>Classe dell'inizializzatore

È necessario eseguire una logica personalizzata per inizializzare il contesto viene utilizzato il database la prima volta. In questo modo i dati di valore di inizializzazione da aggiungere al database in modo che sia possibile visualizzare immediatamente i prodotti e le categorie.

Questa procedura consente di aggiungere una nuovo inizializzatore classe c# per il *modelli* cartella.

1. Creare un'altra `Class` nella *modelli* cartella e denominarlo *ProductDatabaseInitializer.cs*.
2. Sostituire il codice predefinito contenuto nella classe con il codice seguente:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample4.cs)]

Come si può notare nel codice precedente, quando il database viene creato e inizializzato, il `Seed` viene sottoposto a override e impostare proprietà. Quando il `Seed` è impostata, i valori delle categorie e i prodotti vengono usati per popolare il database. Se si tenta di aggiornare i dati di seeding modificando il codice sopra riportato dopo aver creato il database, non visibili tutti gli aggiornamenti quando si esegue l'applicazione Web. Il motivo è il codice precedente Usa un'implementazione del `DropCreateDatabaseIfModelChanges` classe riconoscere se il modello (schema) è stato modificato prima di reimpostare i dati di seeding. Se viene apportata alcuna modifica per il `Category` e `Product` non verranno reinizializzate le classi di entità, il database con i dati di seeding.

> [!NOTE] 
> 
> Se si desidera che il database viene ricreato ogni volta che è stata eseguita l'applicazione, è possibile usare la `DropCreateDatabaseAlways` classe anziché il `DropCreateDatabaseIfModelChanges` classe. Tuttavia per questa serie di esercitazioni usano il `DropCreateDatabaseIfModelChanges` classe.


A questo punto in questa esercitazione, si otterrà un *modelli* cartella con quattro nuove classi e classi di un valore predefinito:

![Creare il livello di accesso ai dati - cartella modelli](create_the_data_access_layer/_static/image3.png)

### <a name="configuring-the-application-to-use-the-data-model"></a>Configurazione dell'applicazione per usare il modello di dati

Dopo aver creato le classi che rappresentano i dati, è necessario configurare l'applicazione a usare le classi. Nel *Global. asax* file, si aggiungere il codice che inizializza il modello. Nel *Web. config* file sono stati aggiunti informazioni che indicano a quali database è l'applicazione verranno usata per archiviare i dati che sono rappresentati dalle classi di dati nuovi. Il *Global. asax* file può essere utilizzato per gestire gli eventi dell'applicazione o i metodi. Il *Web. config* file consente di controllare la configurazione dell'applicazione web ASP.NET.

#### <a name="updating-the-globalasax-file"></a>L'aggiornamento del file Global. asax

Per inizializzare i modelli di dati all'avvio dell'applicazione, si aggiornerà il `Application_Start` gestore nel *Global.asax.cs* file.

> [!NOTE] 
> 
> In Esplora soluzioni, è possibile selezionare i *Global. asax* file o la *Global.asax.cs* file per modificare il *Global.asax.cs* file.


1. Aggiungere il seguente codice evidenziato in giallo per il `Application_Start` metodo nella *Global.asax.cs* file.   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample5.cs?highlight=9-10,22-23)]

> [!NOTE] 
> 
> Il browser deve supportare HTML5 per visualizzare il codice evidenziato in giallo quando si visualizza questa serie di esercitazioni in un browser.


Come illustrato nel codice precedente, all'avvio dell'applicazione, l'applicazione specifica si accede l'inizializzatore che verrà eseguito durante la prima volta che i dati. I due spazi dei nomi aggiuntivi sono necessari per l'accesso di `Database` oggetto e il `ProductDatabaseInitializer` oggetto.

 Modifica del File Web. config 

Sebbene Code First di Entity Framework genera un database per l'utente in un percorso predefinito quando il database viene popolato con dati di seeding, aggiungere le proprie informazioni di connessione all'applicazione ti offre controllo della posizione del database. Si specifica la connessione al database usando una stringa di connessione dell'applicazione *Web. config* file nella radice del progetto. Quando si aggiungono una nuova stringa di connessione, è possibile indirizzare il percorso del database (*wingtiptoys.mdf*) per essere compilato nella directory dei dati dell'applicazione (*App\_dati*), anziché sul valore predefinito posizione. Questa modifica consentirà di individuare ed esaminare il file di database più avanti in questa esercitazione.

1. Nelle **Esplora soluzioni**individuare e aprire il *Web. config* file.
2. Aggiungere la stringa di connessione evidenziata in giallo per il `<connectionStrings>` sezione il *Web. config* file come segue:  

    [!code-xml[Main](create_the_data_access_layer/samples/sample6.xml?highlight=4-7)]

Quando l'applicazione viene eseguita per la prima volta, creerà il database in corrispondenza della posizione specificata dalla stringa di connessione. Ma prima di eseguire l'applicazione, è possibile compilare la soluzione prima di tutto.

## <a name="building-the-application"></a>Compilazione dell'applicazione

Per assicurarsi che tutte le classi e le modifiche all'applicazione Web funzionano, è necessario compilare l'applicazione.

1. Dal **Debug** dal menu **compilazione WingtipToys**.  
 Il **Output** verrà visualizzata la finestra e se verificati errori, viene visualizzato un *ha avuto esito positivo* messaggio.  

    ![Creare il livello di accesso ai dati - Output Windows](create_the_data_access_layer/_static/image4.png)

Se si verifica un errore, controllare nuovamente i passaggi precedenti. Le informazioni contenute nel **Output** finestra indicherà quali file ha un problema e in cui nel file è necessaria una modifica. Queste informazioni consentono di determinare quale parte dei passaggi precedenti devono essere esaminato e risolto nel progetto.

## <a name="summary"></a>Riepilogo

In questa esercitazione della serie è avere creato il modello di dati, nonché, aggiunto il codice che verrà utilizzato per inizializzare ed eseguire il seeding del database. Inoltre, aver configurato l'applicazione a usare i modelli di dati quando viene eseguita l'applicazione.

Nella prossima esercitazione, si sarà aggiornare l'interfaccia utente, aggiungere lo spostamento e recuperare dati dal database. Ciò comporterà il database viene creato automaticamente in base alle classi di entità che è stato creato in questa esercitazione.

## <a name="additional-resources"></a>Risorse aggiuntive

[Panoramica di Entity Framework](https://msdn.microsoft.com/library/bb399567.aspx)   
[Guida per principianti a ADO.NET Entity Framework](https://msdn.microsoft.com/data/ee712907)   
[Codice prima lo sviluppo con Entity Framework](http://www.msteched.com/2010/Europe/DEV212) (video)   
[API Code First relazioni Fluent](https://msdn.microsoft.com/data/hh134698)   
[Annotazioni dei dati per Code First](https://msdn.microsoft.com/data/gg193958)  
[Miglioramenti della produttività per Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)

> [!div class="step-by-step"]
> [Precedente](create-the-project.md)
> [Successivo](ui_and_navigation.md)
