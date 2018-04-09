---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
title: Aggiunta di un nuovo campo al modello di film e di tabella di Database (VB) | Documenti Microsoft
author: Rick-Anderson
description: In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 28970e1b-1845-4015-86ef-121e52a6c397
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 5927b7d977e375881fe618b4b844cbd708023ba1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-new-field-to-the-movie-model-and-database-table-vb"></a>Aggiunta di un nuovo campo al modello di film e di tabella di Database (Visual Basic)
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio. Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito. È possibile installare tutti gli elementi facendo clic sul seguente collegamento: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti utilizzando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supportano runtime + tools)
> 
> Se si utilizza Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul seguente collegamento: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un progetto di Visual Web Developer con codice sorgente VB.NET complemento è disponibile in questo argomento. [Scaricare la versione di Visual Basic.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce c#, passare il [c# versione](../cs/adding-a-new-field.md) di questa esercitazione.


In questa sezione viene apportare alcune modifiche per le classi di modello e informazioni su come è possibile aggiornare lo schema di database affinché corrisponda le modifiche del modello.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Aggiunta di una proprietà Rating al modello Movie

Per iniziare, aggiungere un nuovo `Rating` proprietà esistente `Movie` classe. Aprire il *Movie.cs* file e aggiungere il `Rating` proprietà simile alla seguente:

[!code-vb[Main](adding-a-new-field/samples/sample1.vb)]

L'intero `Movie` classe ora ha un aspetto simile al codice seguente:

[!code-vb[Main](adding-a-new-field/samples/sample2.vb)]

Ricompilare l'applicazione utilizzando il **Debug** &gt; **compilare film** comando di menu.

Ora che è stato aggiornato il `Model` (classe), è anche necessario aggiornare il *\Views\Movies\Index.vbhtml* e *\Views\Movies\Create.vbhtml* consente di visualizzare i modelli per supportare la nuova `Rating`proprietà.

Aprire il<em>\Views\Movies\Index.vbhtml</em> file e aggiungere un `<th>Rating</th>` intestazione di colonna subito dopo il <strong>prezzo</strong> colonna. Aggiungere quindi una `<td>` colonna verso la fine del modello per il rendering di `@item.Rating` valore. Di seguito è riportato l'aggiornato <em>Index.vbhtml</em> modello di visualizzazione è simile a:

[!code-vbhtml[Main](adding-a-new-field/samples/sample3.vbhtml)]

Aprire quindi il *\Views\Movies\Create.vbhtml* file e aggiungere il markup seguente verso la fine del form. Si esegue il rendering di una casella di testo in modo che quando viene creato un nuovo filmato, è possibile specificare una classificazione.

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a>Modello di gestione e le differenze dello Schema di Database

Ora è stato aggiornato il codice dell'applicazione per supportare la nuova `Rating` proprietà.

Eseguire l'applicazione e passare al */Movies* URL. Quando si esegue questa operazione, tuttavia, verrà visualizzato l'errore seguente:

![](adding-a-new-field/_static/image1.png)

Viene visualizzato questo errore perché l'aggiornamento `Movie` classe del modello nell'applicazione è diverso rispetto allo schema di ora il `Movie` tabella del database esistente. Nella tabella del database non è presente una colonna `Rating`.

Per impostazione predefinita, quando si utilizza Code First di Entity Framework per creare automaticamente un database, come in precedenza in questa esercitazione, il primo codice aggiunge una tabella nel database per rilevare se lo schema del database è sincronizzato con le classi di modello da che è stato generato. Se non sono sincronizzati, Entity Framework genera un errore. Questo rende più semplice individuare i problemi in fase di sviluppo che potrebbe essere in caso contrario solo (da errori oscuri) in fase di esecuzione. La funzionalità di controllo di sincronizzazione è quello che determina il messaggio di errore da visualizzare appena individuato.

Esistono due approcci per la risoluzione dell'errore:

1. Fare in modo che Entity Framework elimini e crei di nuovo automaticamente il database in base al nuovo schema di classi del modello. Questo approccio è molto utile durante la fase di sviluppo attivo in un database di test, in quanto consente rapidamente evolversi insieme lo schema del modello e il database. Lo svantaggio, tuttavia, è che si perdono i dati esistenti nel database, in modo si *non* per usare questo approccio in un database di produzione.
2. Modificare esplicitamente lo schema del database esistente in modo che corrisponda alle classi del modello. Il vantaggio di questo approccio è che i dati vengono mantenuti. È possibile apportare questa modifica manualmente o creando uno script di modifica del database.

Per questa esercitazione, verrà utilizzato il primo approccio, è necessario Entity Framework Code First automaticamente ricreare il database ogni volta che viene modificato il modello.

## <a name="automatically-re-creating-the-database-on-model-changes"></a>Ricreare il Database nel modello modifiche automaticamente

Di seguito, aggiornare l'applicazione in modo che il primo codice automaticamente eliminato e ricreato il database in qualsiasi momento si modifica il modello per l'applicazione.

> [!NOTE] 
> 
> **Avviso** è consigliabile abilitare questo approccio di automaticamente eliminando e ricreando il database solo quando si utilizza un database di sviluppo o test, e *mai* in un database di produzione che contiene dati reali. Utilizzarla in un server di produzione può causare la perdita di dati.


In **Esplora soluzioni**, fare clic destro la *modelli* cartella, selezionare **Aggiungi**e quindi selezionare **classe**.

![](adding-a-new-field/_static/image2.png)

Nome della classe &quot;MovieInitializer&quot;. Aggiornamento di `MovieInitializer` classe destinata a contenere il codice seguente:

[!code-vb[Main](adding-a-new-field/samples/sample5.vb)]

La `MovieInitializer` classe specifica che il database utilizzato dal modello deve essere eliminato e ricreato automaticamente se le classi modello cambiano. Il codice include un `Seed` per specificare alcuni dati predefiniti per aggiungere automaticamente al database una volta che ha creato (o ricreato). Ciò fornisce un modo utile per popolare il database con alcuni dati di esempio, senza che sia necessario compilare manualmente ogni volta che si applica un modello di modifica.

Ora che è stato definito il `MovieInitializer` (classe), sarà necessario associare gli in modo che ogni volta che viene eseguita l'applicazione, controlla se le classi di modello sono diverse dallo schema del database. In tal caso, è possibile eseguire l'inizializzatore per ricreare il database per la corrispondenza del modello e quindi popolare il database con i dati di esempio.

Aprire il *Global. asax* file che si trova nella radice del `MvcMovies` progetto:

Il *Global. asax* file contiene la classe che definisce l'intera applicazione per il progetto e contiene un `Application_Start` gestore dell'evento che viene eseguito quando il primo avvio dell'applicazione.

Trovare il `Application_Start` (metodo) e aggiungere una chiamata a `Database.SetInitializer` all'inizio del metodo, come illustrato di seguito:

[!code-vb[Main](adding-a-new-field/samples/sample6.vb)]

Il `Database.SetInitializer` appena aggiunto istruzione indica che il database utilizzando il `MovieDBContext` istanza deve essere automaticamente eliminata e ricreata se lo schema e il database non corrispondono. E come si è visto, anche popolerà il database con i dati di esempio specificato nella `MovieInitializer` classe.

Chiudi il *Global. asax* file.

Eseguire nuovamente l'applicazione e passare il */Movies* URL. All'avvio dell'applicazione, viene rilevato che la struttura del modello non corrisponde più lo schema del database. Ricrea il database in modo che corrisponda alla struttura del modello nuovo automaticamente e consente di popolare il database con i film di esempio:

![7_MyMovieList_SM](adding-a-new-field/_static/image3.png)

Fare clic su di **Crea nuovo** collegamento per aggiungere un nuovo film. Si noti che è possibile aggiungere una classificazione.

[![7_CreateRioII](adding-a-new-field/_static/image5.png)](adding-a-new-field/_static/image4.png)

Scegliere **Crea**. Il nuovo filmato, tra cui la valutazione, viene inserito nei film elenco:

![7_ourNewMovie_SM](adding-a-new-field/_static/image6.png)

In questa sezione è stato illustrato come è possibile modificare gli oggetti del modello e sincronizzare il database con le modifiche. È stato inoltre un modo per popolare un database appena creato con dati di esempio in modo è possibile provare gli scenari. Successivamente, si esaminerà come è possibile aggiungere la logica di convalida più completa per le classi di modello e abilitare alcune regole di business da applicare.

> [!div class="step-by-step"]
> [Precedente](examining-the-edit-methods-and-edit-view.md)
> [Successivo](adding-validation-to-the-model.md)
