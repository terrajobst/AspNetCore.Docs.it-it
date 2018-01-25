---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Accesso ai dati del modello da un Controller | Documenti Microsoft
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 91bfa5fe3c5bd3029b7d7c12c8831e1653fb1d2b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="accessing-your-models-data-from-a-controller"></a>Accesso ai dati del modello da un Controller
====================
Da [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

In questa sezione si creerà un nuovo `MoviesController` classe e scrivere codice che recupera i dati dei film e lo visualizza in browser utilizzando un modello di visualizzazione.

**Compilare l'applicazione** prima di procedere al passaggio successivo. Se l'applicazione, si otterrà un errore durante l'aggiunta di un controller.

In Esplora soluzioni fare doppio clic su di *controller* cartella e quindi fare clic su **Aggiungi**, quindi **Controller**.

![](accessing-your-models-data-from-a-controller/_static/image1.png)

Nel **aggiungere lo scaffolding** nella finestra di dialogo fare clic su **Controller MVC 5 con visualizzazioni, mediante Entity Framework**, quindi fare clic su **Aggiungi**.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- Selezionare **filmato (MvcMovie.Models)** per la classe di modello.
- Selezionare **MovieDBContext (MvcMovie.Models)** per la classe di contesto dati.
- Immettere il nome del Controller **MoviesController**.

 L'immagine seguente mostra la finestra di dialogo completata.  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

Fare clic su **Aggiungi**. (Se si verifica un errore, probabile che non è stata compilare l'applicazione prima di avviare l'aggiunta del controller.) Visual Studio crea i file e cartelle seguenti:

- *Un MoviesController.cs* file nel *controller* cartella.
- Oggetto *Views\Movies* cartella.
- *Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, e *cshtml* nel nuovo *Views\Movies* cartella.

Visual Studio creato automaticamente il [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (creare, leggere, aggiornare ed eliminare) i metodi di azione e le viste per l'utente (la creazione automatica delle viste e i metodi di azione CRUD è noto come scaffolding). Ora è un'applicazione web completamente funzionale che consente di creare, elencare, modificare ed eliminare le voci di film.

Eseguire l'applicazione e fare clic sul **MVC film** collegamento (o selezionare il `Movies` controller aggiungendo */Movies* per l'URL nella barra degli indirizzi del browser). Poiché l'applicazione si basa il routing predefinito (definito nel *App\_Start\RouteConfig.cs* file), la richiesta del browser `http://localhost:xxxxx/Movies` viene indirizzato per il valore predefinito `Index` metodo di azione del `Movies` controller. In altre parole, la richiesta del browser `http://localhost:xxxxx/Movies` corrisponde in modo efficace la richiesta del browser `http://localhost:xxxxx/Movies/Index`. Il risultato è un elenco vuoto di film, perché ancora non sono stati aggiunti.

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a>Creazione di un film

Selezionare il collegamento **Crea nuovo**. Immettere alcune informazioni dettagliate su un filmato, quindi scegliere il **crea** pulsante.


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> Potrebbe non essere in grado di immettere decimali o virgole nel campo Prezzo. Per supportare la convalida jQuery per inglesi che utilizzano una virgola (&quot;,&quot;) per un separatore decimale e formati di data non in lingua inglese Stati Uniti, è necessario includere *globalize.js* specifici e  *Cultures/globalize.Cultures.js* file (da [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) e JavaScript per utilizzare `Globalize.parseFloat`. Illustrato come eseguire questa operazione nella prossima esercitazione. Per il momento, immettere solo numeri interi come 10.


Fare clic su di **crea** pulsante fa sì che il modulo di essere inviata al server, in cui le informazioni di film vengono salvate nel database. Quindi si viene reindirizzati al */Movies* URL, in cui è possibile visualizzare il film appena creato nell'elenco.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Creare altre due voci di filmato. Provare i collegamenti **Modifica**, **Dettagli** e **Elimina**, che sono tutti funzionali.

## <a name="examining-the-generated-code"></a>Analisi del codice generato

Aprire il *Controllers\MoviesController.cs* file ed esaminare generato `Index` metodo. Una parte del controller di film con il `Index` metodo è illustrato di seguito.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Una richiesta per il `Movies` controller restituisce tutte le voci la `Movies` tabella e quindi passa i risultati per il `Index` visualizzazione. La riga seguente dalla `MoviesController` classe crea un'istanza di un contesto di database, film, come descritto in precedenza. È possibile utilizzare il contesto del database film per eseguire una query, modificare ed eliminare i film.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a>Fortemente tipizzate modelli e @model (parola chiave)

In precedenza in questa esercitazione, si è visto come un controller può passare oggetti o dati a un modello di visualizzazione utilizzando il `ViewBag` oggetto. Il `ViewBag` è un oggetto dinamico che fornisce un modo pratico ad associazione tardiva per passare informazioni a una vista.

MVC fornisce inoltre la possibilità di passare *fortemente* un modello di visualizzazione di oggetti tipizzati. Questo approccio fortemente tipizzato consente una migliore in fase di compilazione più ricco e controllo del codice [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) nell'editor di Visual Studio. Il meccanismo di scaffolding in Visual Studio utilizzato questo approccio (vale a dire, passando un *fortemente* modello tipizzato) con il `MoviesController` modelli di classe e di visualizzazione quando creato i metodi e le visualizzazioni.

Nel *Controllers\MoviesController.cs* file esaminare generato `Details` metodo. Il `Details` metodo è illustrato di seguito.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

Il `id` parametro viene in genere passato come dati di route, ad esempio `http://localhost:1234/movies/details/1` imposterà il controller al controller di film, l'azione da `details` e `id` su 1. È inoltre possibile passare l'id con una stringa di query come indicato di seguito:

`http://localhost:1234/movies/details?id=1`

Se un `Movie` viene trovato, un'istanza del `Movie` modello viene passato per il `Details` Vista:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

Esaminare il contenuto del *Views\Movies\Details.cshtml* file:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

Includendo un `@model` istruzione all'inizio del file di modello di visualizzazione, è possibile specificare il tipo di oggetto che prevede la visualizzazione. Al momento della creazione del controller di film, l'istruzione `@model` è stata inclusa automaticamente in Visual Studio all'inizio del file *Details.cshtml*:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Questa direttiva `@model` consente di accedere al film passato dal controller alla vista usando un oggetto `Model` fortemente tipizzato. Ad esempio, nel *Details.cshtml* modello, il codice passa tutti i campi film il `DisplayNameFor` e [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) helper HTML con l'oggetto fortemente tipizzato `Model` oggetto. Il `Create` e `Edit` metodi e i modelli di visualizzazione anche passano un oggetto modello film.

Esaminare il *cshtml* modello di visualizzazione e la `Index` metodo il *MoviesController.cs* file. Si noti come il codice crea un [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) oggetto quando viene chiamata la `View` il metodo di supporto nel `Index` metodo di azione. Il codice passa quindi questo `Movies` elenco dal `Index` il metodo di azione alla visualizzazione:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

Al momento della creazione del controller di film, Visual Studio automaticamente includeva `@model` istruzione all'inizio del *cshtml* file:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

Questo `@model` direttiva consente di accedere all'elenco di film che passato alla visualizzazione per il controller utilizzando un `Model` oggetto fortemente tipizzato. Ad esempio, nel *cshtml* modello, il codice scorre i film in questo modo un `foreach` istruzione tramite l'oggetto fortemente tipizzato `Model` oggetto:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

Poiché il `Model` sono fortemente tipizzato di oggetti (come un `IEnumerable<Movie>` oggetto), ogni `item` oggetto nel ciclo è tipizzato come `Movie`. Tra gli altri vantaggi, ciò significa che ottenere il controllo del codice in fase di compilazione e di supporto di IntelliSense nell'editor di codice completo:

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a>Utilizzo di SQL Server Local DB

Code First di Entity Framework ha rilevato che a cui punta la stringa di connessione di database che è stata fornita un `Movies` database che non esiste ancora, pertanto il primo codice creato il database automaticamente. È possibile verificare che sia stato creato esaminando il *App\_dati* cartella. Se non viene visualizzato il *Movies.mdf* file, fare clic sul **Mostra tutti i file** pulsante il **Esplora** sulla barra degli strumenti, fare clic sul **aggiornare** pulsante e quindi espandere il *App\_dati* cartella.

![](accessing-your-models-data-from-a-controller/_static/image9.png)

Fare doppio clic su *Movies.mdf* per aprire **Esplora SERVER**, quindi espandere il **tabelle** cartella per visualizzare la tabella di film. Si noti l'icona accanto a ID chiave Per impostazione predefinita, EF consentirà una proprietà denominata ID chiave primaria. Per ulteriori informazioni su EF and MVC, vedere ottima esercitazione di Tom Dykstra in [MVC ed EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")

Fare doppio clic su di `Movies` tabella e selezionare **Mostra dati tabella** per visualizzare i dati è stato creato.

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

Fare doppio clic su di `Movies` tabella e selezionare **Apri definizione tabella** per visualizzare la tabella di struttura che Entity Framework Code First creato.

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

Si noti come lo schema del `Movies` esegue il mapping alla tabella il `Movie` classe creata in precedenza. Code First di Entity Framework automaticamente questo schema creato in base al `Movie` classe.

Al termine, chiudere la connessione facendo clic *MovieDBContext* e selezionando **Chiudi connessione**. (Se si non chiude la connessione, si verifichi un errore alla successiva che esecuzione del progetto).

![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")

È ora disponibile un database e alcune pagine per visualizzare, modificare, aggiornare ed eliminare dati. Nella prossima esercitazione, si sarà il resto del codice scaffolding di esaminare e aggiungere un `SearchIndex` (metodo) e un `SearchIndex` visualizzazione che consente di eseguire la ricerca di filmati in questo database. Per ulteriori informazioni sull'utilizzo di Entity Framework con MVC, vedere [la creazione di un modello di dati di Entity Framework per un'applicazione MVC ASP.NET](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

>[!div class="step-by-step"]
[Precedente](creating-a-connection-string.md)
[Successivo](examining-the-edit-methods-and-edit-view.md)
