---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
title: Aggiunge un nuovo campo al modello di filmato e tabella (c#) | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: b4e76c1a-f66e-43a0-aa72-f39df79c07c1
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 3767ca5f0f32c2a93217d902e200fa2dd3dd262a
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578289"
---
<a name="adding-a-new-field-to-the-movie-model-and-table-c"></a>Aggiunge un nuovo campo al modello di filmato e tabella (c#)
====================
da [Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > È disponibile una versione aggiornata di questa esercitazione [qui](../../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e vengono illustrate altre funzionalità.
> 
> 
> Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio. Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito. È possibile installare tutti gli elementi facendo clic sul collegamento seguente: [instalace Webové Platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime e strumenti di supportano)
> 
> Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul collegamento seguente: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un progetto di Visual Web Developer con codice sorgente c# è disponibile a complemento di questo argomento. [Scaricare la versione c#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce Visual Basic, passare al [versione Visual Basic](../vb/intro-to-aspnet-mvc-3.md) di questa esercitazione.


In questa sezione verrà apportare alcune modifiche alle classi del modello e informazioni su come è possibile aggiornare lo schema del database in modo da corrispondere le modifiche al modello.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Aggiunta di una proprietà Rating al modello Movie

Iniziare aggiungendo una nuova `Rating` proprietà esistente `Movie` classe. Aprire il *Movie.cs* file e aggiungere il `Rating` proprietà simile alla seguente:

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

L'intero `Movie` classe avrà l'aspetto simile al codice seguente:

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

Ricompilare l'applicazione usando il **Debug** &gt; **compilare film** comando di menu.

Ora che è stato aggiornato il `Model` (classe), è anche necessario aggiornare il *\Views\Movies\Index.cshtml* e *\Views\Movies\Create.cshtml* consente di visualizzare i modelli per supportare la nuova `Rating`proprietà.

Aprire il *\Views\Movies\Index.cshtml* file e aggiungere un `<th>Rating</th>` intestazione di colonna subito dopo il **prezzo** colonna. Aggiungere quindi una `<td>` colonna verso la fine del modello per eseguire il rendering di `@item.Rating` valore. Ecco quali aggiornato *index. cshtml* modello di visualizzazione è simile a:

[!code-cshtml[Main](adding-a-new-field/samples/sample3.cshtml)]

Successivamente, aprire il *\Views\Movies\Create.cshtml* file e aggiungere il markup seguente verso la fine del form. Si esegue il rendering di una casella di testo in modo che sia possibile specificare una classificazione uguale a quando viene creato un nuovo film.

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a>Modello di gestione e differenze di Schema di Database

A questo punto è stato aggiornato il codice dell'applicazione per supportare la nuova `Rating` proprietà.

Ora eseguire l'applicazione e passare al */Movies* URL. Quando si esegue questa operazione, tuttavia, si noterà l'errore seguente:

![](adding-a-new-field/_static/image1.png)

Viene visualizzato questo errore perché aggiornato `Movie` classe di modello nell'applicazione ora è diverso rispetto allo schema del `Movie` tabella del database esistente. Nella tabella del database non è presente una colonna `Rating`.

Per impostazione predefinita, quando si utilizza Code First di Entity Framework per creare automaticamente un database, come è stato fatto in precedenza in questa esercitazione, Code First consente di aggiungere una tabella nel database per rilevare se lo schema del database è sincronizzato con le classi del modello da che è stato generato. Se non sono sincronizzati, Entity Framework genera un errore. Questo rende più semplice individuare i problemi in fase di sviluppo che potrebbero altrimenti solo risultare (da errori sconosciuti) in fase di esecuzione. La funzionalità di controllo della sincronizzazione è ciò che il messaggio di errore da visualizzare i risultati che abbiamo appena visto.

Esistono due approcci per la risoluzione dell'errore:

1. Fare in modo che Entity Framework elimini e crei di nuovo automaticamente il database in base al nuovo schema di classi del modello. Questo approccio è molto utile quando si esegue lo sviluppo attivo in un database di prova, perché consente di migliorare rapidamente lo schema di modello e il database insieme. Lo svantaggio, tuttavia, è che si perdono i dati esistenti nel database, in modo che è *non* vuole usare questo approccio in un database di produzione.
2. Modificare esplicitamente lo schema del database esistente in modo che corrisponda alle classi del modello. Il vantaggio di questo approccio è che i dati vengono mantenuti. È possibile apportare questa modifica manualmente o creando uno script di modifica del database.

Per questa esercitazione si userà il primo approccio, è possibile Code First di Entity Framework crei di nuovo automaticamente il database ogni volta che viene modificato il modello.

## <a name="automatically-re-creating-the-database-on-model-changes"></a>Ricreare automaticamente il Database su modifiche al modello

È possibile aggiornare l'applicazione in modo che Code First automaticamente viene eliminato e ricreato il database ogni volta che si modifica il modello per l'applicazione.

> [!NOTE] 
> 
> **Avviso** è consigliabile abilitare questo approccio di automaticamente eliminare e ricreare il database solo quando si usa un database di sviluppo o test, e *mai* in un database di produzione che contiene dati reali. Usarla in un server di produzione può causare la perdita di dati.


Nella **Esplora soluzioni**, fare clic il *modelli* cartella, selezionare **Add**e quindi selezionare **classe**.

![](adding-a-new-field/_static/image2.png)

Denominare la classe "MovieInitializer". Aggiornamento di `MovieInitializer` classe destinata a contenere il codice seguente:

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

Il `MovieInitializer` classe specifica che il database utilizzato dal modello deve essere eliminato e ricreato automaticamente se cambiano le classi del modello. Il codice include un `Seed` metodo per specificare alcuni dati predefiniti per aggiungere automaticamente al database una volta che ha creato (o ricreato). Ciò fornisce un modo utile per popolare il database con alcuni dati di esempio, senza che sia necessario popolarlo manualmente ogni volta che si esegue un modello di modifica.

Ora che sono stati definiti i `MovieInitializer` (classe), è opportuno collegare i in modo che ogni volta che viene eseguita l'applicazione, controlla se le classi del modello sono diverse dallo schema del database. In tal caso, è possibile eseguire l'inizializzatore per ricreare il database per corrispondere al modello e popolare il database con i dati di esempio.

Aprire il *Global. asax* file che si trova nella radice del `MvcMovies` progetto:

[![](adding-a-new-field/_static/image4.png)](adding-a-new-field/_static/image3.png)

Il *Global. asax* file contiene la classe che definisce l'intera applicazione per il progetto e contiene un `Application_Start` gestore dell'evento che viene eseguito quando il primo avvio dell'applicazione.

Aggiungiamo due istruzioni using all'inizio del file. Il primo fa riferimento lo spazio dei nomi di Entity Framework e il secondo fa riferimento a spazio dei nomi in cui il `MovieInitializer` vite di classe:

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs)]

Trovare quindi il `Application_Start` metodo e aggiungere una chiamata a `Database.SetInitializer` all'inizio del metodo, come illustrato di seguito:

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs)]

Il `Database.SetInitializer` istruzione appena aggiunta indica che il database utilizzando la `MovieDBContext` istanza deve essere automaticamente eliminata e ricreata se lo schema e il database non corrispondono. E come si è visto, verrà inoltre popolato il database con i dati di esempio che viene specificati nel `MovieInitializer` classe.

Chiudi il *Global. asax* file.

Eseguire nuovamente l'applicazione e passare al */Movies* URL. All'avvio dell'applicazione, rileva che la struttura del modello non corrisponde più lo schema del database. Automaticamente ricrea il database in modo da corrispondere la nuova struttura di modello e lo popola con i film di esempio:

![7_MyMovieList_SM](adding-a-new-field/_static/image5.png)

Scegliere il **Crea nuovo** collegamento per aggiungere un nuovo film. Si noti che è possibile aggiungere una classificazione.

[![7_CreateRioII](adding-a-new-field/_static/image7.png)](adding-a-new-field/_static/image6.png)

Scegliere **Crea**. Questo nuovo film, tra cui la classificazione, ora viene visualizzata nell'elenco di film:

[![7_ourNewMovie_SM](adding-a-new-field/_static/image9.png)](adding-a-new-field/_static/image8.png)

In questa sezione è stato illustrato come è possibile modificare gli oggetti modello e sincronizzare il database con le modifiche. Si è appreso anche un modo per popolare un database appena creato con dati di esempio in modo che è possibile provare gli scenari. Successivamente, diamo un'occhiata a come è possibile aggiungere più completa della logica di convalida alle classi di modello e abilitare alcune regole di business possano essere applicate.

> [!div class="step-by-step"]
> [Precedente](examining-the-edit-methods-and-edit-view.md)
> [Successivo](adding-validation-to-the-model.md)
