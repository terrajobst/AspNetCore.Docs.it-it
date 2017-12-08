---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
title: Accesso ai dati del modello da un Controller (VB) | Documenti Microsoft
author: Rick-Anderson
description: "In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: cad00de1-3c68-4ff4-a436-54236d449459
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: d0c6976519f4f4bae10fabf4cbf85401de4f58e7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="accessing-your-models-data-from-a-controller-vb"></a>Accesso ai dati del modello da un Controller (VB)
====================
Da [Rick Anderson](https://github.com/Rick-Anderson)

> In questa esercitazione verrà fornite le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, che è una versione gratuita di Microsoft Visual Studio. Prima di iniziare, assicurarsi di che aver installato i prerequisiti elencati di seguito. È possibile installare tutti gli elementi facendo clic sul seguente collegamento: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti utilizzando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supportano runtime + tools)
> 
> Se si utilizza Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti, fare clic sul seguente collegamento: [prerequisiti di Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un progetto di Visual Web Developer con codice sorgente VB.NET complemento è disponibile in questo argomento. [Scaricare la versione di Visual Basic.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce c#, passare il [c# versione](../cs/accessing-your-models-data-from-a-controller.md) di questa esercitazione.


In questa sezione si creerà un nuovo `MoviesController` classe e scrivere codice che recupera i dati dei film e lo visualizza in browser utilizzando un modello di visualizzazione. Assicurarsi di compilare l'applicazione prima di procedere.

Fare doppio clic su di *controller* cartella e creare un nuovo `MoviesController` controller. Selezionare le opzioni seguenti:

- Nome del controller: **MoviesController**. (Questo è il valore predefinito).
- Modello: **Controller con azioni di lettura/scrittura e visualizzazioni, mediante Entity Framework**.
- Classe del modello: **filmato (MvcMovie.Models)**.
- Classe del contesto dati: **MovieDBContext (MvcMovie.Models)**.
- Visualizzazioni: **Razor (CSHTML)**. (Predefinito).

[![5addMovieController](accessing-your-models-data-from-a-controller/_static/image2.png)](accessing-your-models-data-from-a-controller/_static/image1.png)

Fare clic su **Aggiungi**. Visual Web Developer crea i file e cartelle seguenti:

- *Un MoviesController.vb* nel file del progetto *controller* cartella.
- Oggetto *filmati* cartella del progetto *viste* cartella.
- *Create.vbhtml, Delete.vbhtml, Details.vbhtml, Edit.vbhtml*, e *Index.vbhtml* nel nuovo *Views\Movies* cartella.

[![5_ScaffoldMovie](accessing-your-models-data-from-a-controller/_static/image4.png)](accessing-your-models-data-from-a-controller/_static/image3.png)

Il meccanismo di scaffolding di ASP.NET MVC 3 creato automaticamente il CRUD (creare, leggere, aggiornare ed eliminare) i metodi di azione e le viste per l'utente. Ora è un'applicazione web completamente funzionale che consente di creare, elencare, modificare ed eliminare le voci di film.

Eseguire l'applicazione e individuare il `Movies` controller aggiungendo */Movies* per l'URL nella barra degli indirizzi del browser. Poiché l'applicazione si basa il routing predefinito (definito nel *Global. asax* file), la richiesta del browser `http://localhost:xxxxx/Movies` viene indirizzato per il valore predefinito `Index` metodo di azione del `Movies` controller. In altre parole, la richiesta del browser `http://localhost:xxxxx/Movies` corrisponde in modo efficace la richiesta del browser `http://localhost:xxxxx/Movies/Index`. Il risultato è un elenco vuoto di film, perché ancora non sono stati aggiunti.

![](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="creating-a-movie"></a>Creazione di un film

Selezionare il collegamento **Crea nuovo**. Immettere alcune informazioni dettagliate su un filmato, quindi scegliere il **crea** pulsante.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Fare clic su di **crea** pulsante fa sì che il modulo di essere inviata al server, in cui le informazioni di film vengono salvate nel database. Quindi si viene reindirizzati al */Movies* URL, in cui è possibile visualizzare il film appena creato nell'elenco.

[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png)](accessing-your-models-data-from-a-controller/_static/image7.png)

Creare altre due voci di filmato. Provare i collegamenti **Modifica**, **Dettagli** e **Elimina**, che sono tutti funzionali.

## <a name="examining-the-generated-code"></a>Analisi del codice generato

Aprire il *Controllers\MoviesController.vb* file ed esaminare generato `Index` metodo. Una parte del controller di film con il `Index` metodo è illustrato di seguito.

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample1.vb)]

La riga seguente dalla `MoviesController` classe crea un'istanza di un contesto di database, film, come descritto in precedenza. È possibile utilizzare il contesto del database film per eseguire una query, modificare ed eliminare i film.

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample2.vb)]

Una richiesta per il `Movies` controller restituisce tutte le voci la `Movies` tabella del database film e quindi passa i risultati per il `Index` visualizzazione.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Fortemente tipizzate modelli e @model (parola chiave)

In precedenza in questa esercitazione, si è visto come un controller può passare oggetti o dati a un modello di visualizzazione utilizzando il `ViewBag` oggetto. Il `ViewBag` è un oggetto dinamico che fornisce un modo pratico ad associazione tardiva per passare informazioni a una vista.

ASP.NET MVC fornisce inoltre la possibilità di passare fortemente tipizzata di dati o oggetti da un modello di visualizzazione. Questo approccio consente una migliore in fase di compilazione il controllo del codice e più completa IntelliSense nell'editor di Visual Web Developer è fortemente tipizzato. Si utilizza questo approccio che prevede il `MoviesController` classe e *Index.vbhtml* modello di visualizzazione.

Si noti come il codice crea un [ `List` ](https://msdn.microsoft.com/en-us/library/6sh2ey19.aspx) oggetto quando viene chiamata la `View` il metodo di supporto nel `Index` metodo di azione. Il codice passa quindi questo `Movies` elenco dal controller alla visualizzazione:

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample3.vb)]

Includendo un `@ModelType` istruzione all'inizio del file di modello di visualizzazione, è possibile specificare il tipo di oggetto che prevede la visualizzazione. Al momento della creazione del controller di film, Visual Web Developer viene incluso automaticamente le operazioni seguenti `@model` istruzione all'inizio del *Index.vbhtml* file:

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.vbhtml)]

Questo `@ModelType` direttiva consente di accedere all'elenco di film che passato alla visualizzazione per il controller utilizzando un `Model` oggetto fortemente tipizzato. Ad esempio, nel *Index.vbhtml* modello, il codice scorre i film in questo modo un `foreach` istruzione tramite l'oggetto fortemente tipizzato `Model` oggetto:

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.vbhtml)]

Poiché il `Model` sono fortemente tipizzato di oggetti (come un `IEnumerable<Movie>` oggetto), ogni `item` oggetto nel ciclo è tipizzato come `Movie`. Tra gli altri vantaggi, ciò significa che ottenere il controllo del codice in fase di compilazione e di supporto di IntelliSense nell'editor di codice completo:

[![5_Intellisense](accessing-your-models-data-from-a-controller/_static/image10.png)](accessing-your-models-data-from-a-controller/_static/image9.png)

## <a name="working-with-sql-server-compact"></a>Utilizzo di SQL Server Compact

Code First di Entity Framework ha rilevato che a cui punta la stringa di connessione di database che è stata fornita un `Movies` database che non esiste ancora, pertanto il primo codice creato il database automaticamente. È possibile verificare che sia stato creato esaminando il *App\_dati* cartella. Se non viene visualizzato il *Movies.sdf* file, fare clic sul **Mostra tutti i file** pulsante il **Esplora** sulla barra degli strumenti, fare clic sul **aggiornare** pulsante e quindi espandere il *App\_dati* cartella.

[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png)](accessing-your-models-data-from-a-controller/_static/image11.png)

Fare doppio clic su *Movies.sdf* per aprire **Esplora Server**. Espandere quindi la **tabelle** cartella per visualizzare le tabelle che sono state create nel database.

> [!NOTE]
> Se si verifica un errore quando si fa doppio clic *Movies.sdf*, verificare di aver installato **Visual Studio 2010 SP1 Tools per SQL Server Compact 4.0**. (Per i collegamenti al software, vedere l'elenco dei prerequisiti nella parte 1 di questa serie di esercitazioni). Se si installa la versione, è necessario chiudere e riaprire Visual Web Developer.


[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png)](accessing-your-models-data-from-a-controller/_static/image13.png)

Esistono due tabelle, una per il `Movie` set di entità e quindi la `EdmMetadata` tabella. Il `EdmMetadata` tabella viene utilizzata da Entity Framework per determinare quando il modello e il database non sono sincronizzati.

Fare doppio clic su di `Movies` tabella e selezionare **Mostra dati tabella** per visualizzare i dati è stato creato.

[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png)](accessing-your-models-data-from-a-controller/_static/image15.png)

Fare doppio clic su di `Movies` tabella e selezionare **modifica Schema tabella**.

[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png)](accessing-your-models-data-from-a-controller/_static/image17.png)

![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image19.png)

Si noti come lo schema del `Movies` esegue il mapping alla tabella il `Movie` classe creata in precedenza. Code First di Entity Framework automaticamente questo schema creato in base al `Movie` classe.

Al termine, chiudere la connessione. (Se si non chiude la connessione, si verifichi un errore alla successiva che esecuzione del progetto).

[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image21.png)](accessing-your-models-data-from-a-controller/_static/image20.png)

È ora disponibile il database e una pagina di elenco semplice per visualizzare il contenuto da esso. Nella prossima esercitazione, si sarà il resto del codice scaffolding di esaminare e aggiungere un `SearchIndex` (metodo) e un `SearchIndex` visualizzazione che consente di eseguire la ricerca di filmati in questo database.

>[!div class="step-by-step"]
[Precedente](adding-a-model.md)
[Successivo](examining-the-edit-methods-and-edit-view.md)
