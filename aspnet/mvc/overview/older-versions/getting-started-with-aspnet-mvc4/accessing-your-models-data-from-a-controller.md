---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: Accesso ai dati del modello da un Controller | Documenti Microsoft
author: Rick-Anderson
description: "Nota: Una versione aggiornata di questa esercitazione è disponibile qui che utilizza ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice seguire e demo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: f323fe37da739d957a609dc7ca4e71a3c3ab475e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="accessing-your-models-data-from-a-controller"></a>Accesso ai dati del modello da un Controller
====================
Da [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > È disponibile una versione aggiornata di questa esercitazione [qui](../../getting-started/introduction/getting-started.md) che utilizza ASP.NET MVC 5 e Visual Studio 2013. È molto più semplice da seguire, più sicuro e vengono illustrate altre funzionalità.


In questa sezione si creerà un nuovo `MoviesController` classe e scrivere codice che recupera i dati dei film e lo visualizza in browser utilizzando un modello di visualizzazione.

**Compilare l'applicazione** prima di procedere al passaggio successivo.

Fare doppio clic su di *controller* cartella e creare un nuovo `MoviesController` controller. Le opzioni seguenti non verranno visualizzati fino a quando non si compila l'applicazione. Selezionare le opzioni seguenti:

- Nome del controller: **MoviesController**. (Questo è il valore predefinito. )
- Modello: **Controller MVC con azioni di lettura/scrittura e visualizzazioni, mediante Entity Framework**.
- Classe del modello: **filmato (MvcMovie.Models)**.
- Classe del contesto dati: **MovieDBContext (MvcMovie.Models)**.
- Visualizzazioni: **Razor (CSHTML)**. (Predefinito).

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

Fare clic su **Aggiungi**. Visual Studio Express crea i file e cartelle seguenti:

- *Un MoviesController.cs* nel file del progetto *controller* cartella.
- Oggetto *filmati* cartella del progetto *viste* cartella.
- *Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, e *cshtml* nel nuovo *Views\Movies* cartella.

ASP.NET MVC 4 creato automaticamente il CRUD (creare, leggere, aggiornare ed eliminare) i metodi di azione e le viste per l'utente (la creazione automatica delle viste e i metodi di azione CRUD è noto come scaffolding). Ora è un'applicazione web completamente funzionale che consente di creare, elencare, modificare ed eliminare le voci di film.

Eseguire l'applicazione e individuare il `Movies` controller aggiungendo */Movies* per l'URL nella barra degli indirizzi del browser. Poiché l'applicazione si basa il routing predefinito (definito nel *Global. asax* file), la richiesta del browser `http://localhost:xxxxx/Movies` viene indirizzato per il valore predefinito `Index` metodo di azione del `Movies` controller. In altre parole, la richiesta del browser `http://localhost:xxxxx/Movies` corrisponde in modo efficace la richiesta del browser `http://localhost:xxxxx/Movies/Index`. Il risultato è un elenco vuoto di film, perché ancora non sono stati aggiunti.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a>Creazione di un film

Selezionare il collegamento **Crea nuovo**. Immettere alcune informazioni dettagliate su un filmato, quindi scegliere il **crea** pulsante.

![](accessing-your-models-data-from-a-controller/_static/image3.png)

Fare clic su di **crea** pulsante fa sì che il modulo di essere inviata al server, in cui le informazioni di film vengono salvate nel database. Quindi si viene reindirizzati al */Movies* URL, in cui è possibile visualizzare il film appena creato nell'elenco.

![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")

Creare altre due voci di filmato. Provare i collegamenti **Modifica**, **Dettagli** e **Elimina**, che sono tutti funzionali.

## <a name="examining-the-generated-code"></a>Analisi del codice generato

Aprire il *Controllers\MoviesController.cs* file ed esaminare generato `Index` metodo. Una parte del controller di film con il `Index` metodo è illustrato di seguito.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

La riga seguente dalla `MoviesController` classe crea un'istanza di un contesto di database, film, come descritto in precedenza. È possibile utilizzare il contesto del database film per eseguire una query, modificare ed eliminare i film.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Una richiesta per il `Movies` controller restituisce tutte le voci la `Movies` tabella del database film e quindi passa i risultati per il `Index` visualizzazione.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Fortemente tipizzate modelli e @model (parola chiave)

In precedenza in questa esercitazione, si è visto come un controller può passare oggetti o dati a un modello di visualizzazione utilizzando il `ViewBag` oggetto. Il `ViewBag` è un oggetto dinamico che fornisce un modo pratico ad associazione tardiva per passare informazioni a una vista.

ASP.NET MVC fornisce inoltre la possibilità di passare fortemente tipizzata di dati o oggetti da un modello di visualizzazione. Questo approccio consente una migliore in fase di compilazione il controllo del codice e più completa IntelliSense nell'editor di Visual Studio è fortemente tipizzato. Il meccanismo di scaffolding in Visual Studio utilizzato questo approccio che prevede il `MoviesController` modelli di classe e di visualizzazione quando creato i metodi e le visualizzazioni.

Nel *Controllers\MoviesController.cs* file esaminare generato `Details` metodo. Una parte del controller di film con il `Details` metodo è illustrato di seguito.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

Se un `Movie` viene trovato, un'istanza di `Movie` modello viene passato alla visualizzazione dettagli. Esaminare il contenuto del *Views\Movies\Details.cshtml* file.

Includendo un `@model` istruzione all'inizio del file di modello di visualizzazione, è possibile specificare il tipo di oggetto che prevede la visualizzazione. Al momento della creazione del controller di film, l'istruzione `@model` è stata inclusa automaticamente in Visual Studio all'inizio del file *Details.cshtml*:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

Questa direttiva `@model` consente di accedere al film passato dal controller alla vista usando un oggetto `Model` fortemente tipizzato. Ad esempio, nel *Details.cshtml* modello, il codice passa tutti i campi film il `DisplayNameFor` e [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) helper HTML con l'oggetto fortemente tipizzato `Model` oggetto. I metodi di creazione e modifica e i modelli di visualizzazione anche passano un oggetto modello film.

Esaminare il *cshtml* modello di visualizzazione e la `Index` metodo il *MoviesController.cs* file. Si noti come il codice crea un [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) oggetto quando viene chiamata la `View` il metodo di supporto nel `Index` metodo di azione. Il codice passa quindi questo `Movies` elenco dal controller alla visualizzazione:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

Al momento della creazione del controller di film, Visual Studio Express automaticamente includeva `@model` istruzione all'inizio del *cshtml* file:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Questo `@model` direttiva consente di accedere all'elenco di film che passato alla visualizzazione per il controller utilizzando un `Model` oggetto fortemente tipizzato. Ad esempio, nel *cshtml* modello, il codice scorre i film in questo modo un `foreach` istruzione tramite l'oggetto fortemente tipizzato `Model` oggetto:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

Poiché il `Model` sono fortemente tipizzato di oggetti (come un `IEnumerable<Movie>` oggetto), ogni `item` oggetto nel ciclo è tipizzato come `Movie`. Tra gli altri vantaggi, ciò significa che ottenere il controllo del codice in fase di compilazione e di supporto di IntelliSense nell'editor di codice completo:

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a>Utilizzo di SQL Server Local DB

Code First di Entity Framework ha rilevato che a cui punta la stringa di connessione di database che è stata fornita un `Movies` database che non esiste ancora, pertanto il primo codice creato il database automaticamente. È possibile verificare che sia stato creato esaminando il *App\_dati* cartella. Se non viene visualizzato il *Movies.mdf* file, fare clic sul **Mostra tutti i file** pulsante il **Esplora** sulla barra degli strumenti, fare clic sul **aggiornare** pulsante e quindi espandere il *App\_dati* cartella.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Fare doppio clic su *Movies.mdf* per aprire **Esplora DATABASE**, quindi espandere il **tabelle** cartella per visualizzare la tabella di film.

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")

> [!NOTE]
> Se non viene visualizzato Esplora database, dal **strumenti** dal menu **Connetti al Database**, quindi annullare il **Scegli origine dati** finestra di dialogo. Questa operazione forzerà aprire Esplora database.


> [!NOTE]
> Se si utilizza VWD o Visual Studio 2010 e si verifica un errore simile a uno dei seguenti seguenti:
> 
> - Il database ' C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES. MDF' non può essere aperto perché la versione 706 è. Questo server supporta 655 e nelle versioni precedenti. Un percorso per il downgrade non è supportato.
> - &quot;Non è stata gestita dal codice utente eccezione InvalidOperation&quot; nella stringa SqlConnection fornita non specifica un catalogo iniziale.
> 
> È necessario installare il [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) e [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0). Verificare il `MovieDBContext` stringa di connessione specificata nella pagina precedente.


Fare doppio clic su di `Movies` tabella e selezionare **Mostra dati tabella** per visualizzare i dati è stato creato.

![](accessing-your-models-data-from-a-controller/_static/image8.png)

Fare doppio clic su di `Movies` tabella e selezionare **Apri definizione tabella** per visualizzare la tabella di struttura che Entity Framework Code First creato.

![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")

![](accessing-your-models-data-from-a-controller/_static/image10.png)

Si noti come lo schema del `Movies` esegue il mapping alla tabella il `Movie` classe creata in precedenza. Code First di Entity Framework automaticamente questo schema creato in base al `Movie` classe.

Al termine, chiudere la connessione facendo clic *MovieDBContext* e selezionando **Chiudi connessione**. (Se si non chiude la connessione, si verifichi un errore alla successiva che esecuzione del progetto).

![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")

È ora disponibile il database e una pagina di elenco semplice per visualizzare il contenuto da esso. Nella prossima esercitazione, si sarà il resto del codice scaffolding di esaminare e aggiungere un `SearchIndex` (metodo) e un `SearchIndex` visualizzazione che consente di eseguire la ricerca di filmati in questo database.

>[!div class="step-by-step"]
[Precedente](adding-a-model.md)
[Successivo](examining-the-edit-methods-and-edit-view.md)
