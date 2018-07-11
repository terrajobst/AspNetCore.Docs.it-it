---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Accesso ai dati del modello da un Controller | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: a3f3f4a030650ff65b070528c5efa1605be764a0
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38120153"
---
<a name="accessing-your-models-data-from-a-controller"></a>Accesso ai dati del modello da un Controller
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

In questa sezione si creerà un nuovo `MoviesController` classe e scrivere il codice che recupera i dati dei film e lo visualizza nel browser usando un modello di vista.

**Compilare l'applicazione** prima di procedere al passaggio successivo. Se si non compila l'applicazione, si otterrà un errore durante l'aggiunta di un controller.

In Esplora soluzioni fare doppio clic il *controller* cartella e quindi fare clic su **Add**, quindi **Controller**.

![](accessing-your-models-data-from-a-controller/_static/image1.png)

Nel **Add Scaffold** della finestra di dialogo fare clic su **Controller MVC 5 con visualizzazioni, mediante Entity Framework**, quindi fare clic su **Aggiungi**.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- Selezionare **Movie (mvcmovie. Models)** per la classe di modello.
- Selezionare **MovieDBContext (mvcmovie. Models)** per la classe del contesto dati.
- Immettere il nome Controller **MoviesController**.

  L'immagine seguente mostra la finestra di dialogo completato.  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

Fare clic su **Aggiungi**. (Se si verifica un errore, probabilmente non è stata creata l'applicazione prima di avviare l'aggiunta del controller.) Visual Studio crea il file e cartelle seguenti:

- *Un MoviesController.cs* del file nei *controller* cartella.
- Oggetto *viste\filmati* cartella.
- *Create. cshtml, cshtml, Details. cshtml, Edit. cshtml*, e *index. cshtml* nella nuova *viste\filmati* cartella.

Visual Studio creato automaticamente il [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (creare, leggere, aggiornare ed eliminare) i metodi di azione e visualizzazioni per l'utente (la creazione automatica di metodi di azione CRUD e le visualizzazioni è nota come scaffolding). Ora disponibile un'applicazione web completamente funzionali che consente di creare, elencare, modificare ed eliminare le voci di filmato.

Eseguire l'applicazione e fare clic sulla **MVC Movie** collegamento (o selezionare il `Movies` controller mediante l'aggiunta */Movies* all'URL nella barra degli indirizzi del browser). Poiché l'applicazione si basa il routing predefinito (definito nel *App\_Start\RouteConfig.cs* file), la richiesta del browser `http://localhost:xxxxx/Movies` viene instradato sul valore predefinito `Index` metodo di azione del `Movies` controller. In altre parole, la richiesta del browser `http://localhost:xxxxx/Movies` corrisponde effettivamente alla richiesta del browser `http://localhost:xxxxx/Movies/Index`. Il risultato è un elenco vuoto di film, perché ancora stato aggiunto uno.

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a>Creazione di un film

Selezionare il collegamento **Crea nuovo**. Immettere alcuni dettagli sui filmati, quindi scegliere il **Create** pulsante.


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> Potrebbe non essere in grado di immettere i separatori decimali o virgole nel campo di prezzo. Per supportare la convalida di jQuery per impostazioni locali di lingua diversa dall'inglese che usano la virgola (&quot;,&quot;) per un separatore decimale e formati di data non in lingua inglese Stati Uniti, è necessario includere *globalize.js* specifiche e  *Cultures/globalize.Cultures.js* file (da [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) e JavaScript usare `Globalize.parseFloat`. Verrà illustrato come eseguire questa operazione nella prossima esercitazione. Per il momento, immettere solo numeri interi come 10.


Facendo clic sui **Create** pulsante fa sì che il modulo viene inviato al server, in cui le informazioni sui film viene salvati nel database. Si verrà quindi reindirizzati per il */Movies* URL, in cui è possibile visualizzare i film appena creato nell'elenco.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Creare altre due voci di filmato. Provare i collegamenti **Modifica**, **Dettagli** e **Elimina**, che sono tutti funzionali.

## <a name="examining-the-generated-code"></a>Analisi del codice generato

Aprire il *Controllers\MoviesController.cs* del file ed esaminare il generato `Index` (metodo). Una parte del controller di film con la `Index` metodo è illustrato di seguito.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Una richiesta per il `Movies` controller restituisce tutte le voci la `Movies` tabella e quindi passa i risultati per il `Index` visualizzazione. La riga seguente dal `MoviesController` classe crea un'istanza di un contesto di database di film, come descritto in precedenza. È possibile utilizzare il contesto di database di film per eseguire una query, modificare ed eliminare i film.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modelli fortemente tipizzati e @model (parola chiave)

In precedenza in questa esercitazione si è visto come un controller può passare oggetti o dati a un modello di visualizzazione usando la `ViewBag` oggetto. Il `ViewBag` è un oggetto dinamico che fornisce un modo pratico con associazione tardiva per passare informazioni a una vista.

MVC offre anche la possibilità di passare *fortemente* tipizzata di oggetti da un modello di vista. Questo approccio fortemente tipizzato consente una migliore in fase di compilazione più ricco e controllo del codice [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) nell'editor di Visual Studio. Questo approccio utilizzato il meccanismo di scaffolding in Visual Studio (vale a dire, passando un *fortemente* modello tipizzato) con il `MoviesController` i modelli di classe e visualizzazione della creazione di metodi e visualizzazioni.

Nel *Controllers\MoviesController.cs* file esaminare generato `Details` (metodo). Il `Details` metodo è illustrato di seguito.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

Il `id` parametro viene in genere passato come dati della route, ad esempio `http://localhost:1234/movies/details/1` imposterà il controller al controller di film, l'azione che deve `details` e il `id` su 1. È inoltre possibile passare l'id con una stringa di query come indicato di seguito:

`http://localhost:1234/movies/details?id=1`

Se un `Movie` viene trovato, un'istanza del `Movie` modello viene passato al `Details` Vista:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

Esaminare il contenuto del *Views\Movies\Details.cshtml* file:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

Includendo un `@model` informativa nella parte superiore del file di modello della vista, è possibile specificare il tipo di oggetto previsto dalla vista. Al momento della creazione del controller di film, l'istruzione `@model` è stata inclusa automaticamente in Visual Studio all'inizio del file *Details.cshtml*:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Questa direttiva `@model` consente di accedere al film passato dal controller alla vista usando un oggetto `Model` fortemente tipizzato. Ad esempio, nelle *Details. cshtml* modello, il codice passa ogni campo di film per il `DisplayNameFor` e [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) helper HTML con l'oggetto fortemente tipizzato `Model` oggetto. Il `Create` e `Edit` metodi e modelli di visualizzazione anche passano un oggetto modello di film.

Esaminare i *index. cshtml* modello di visualizzazione e il `Index` metodo nel *MoviesController.cs* file. Si noti che il codice crea un [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) dell'oggetto quando viene chiamato il `View` metodo helper nel `Index` metodo di azione. Il codice passa quindi questo `Movies` dall'elenco il `Index` metodo di azione per la visualizzazione:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

Quando si crea il controller di film, Visual Studio inclusa automaticamente quanto segue `@model` istruzione all'inizio del *index. cshtml* file:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

Ciò `@model` direttiva consente di accedere all'elenco di film che il controller ha passato alla vista usando un `Model` oggetto fortemente tipizzato. Ad esempio, nelle *index. cshtml* modello, il codice scorre i film in questo modo un `foreach` istruzione sull'oggetto fortemente tipizzato `Model` oggetto:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

Poiché il `Model` oggetto è fortemente tipizzato (come un `IEnumerable<Movie>` oggetto), ognuno `item` oggetto nel ciclo viene tipizzato come `Movie`. Tra gli altri vantaggi, ciò significa che si ottiene controllo in fase di compilazione del codice e supporto di IntelliSense nell'editor di codice completo:

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a>Utilizzo di SQL Server Local DB

Code First di Entity Framework ha rilevato che la stringa di connessione di database che è stata fornita puntata un `Movies` database che non esiste ancora, in modo da Code First ha creato il database automaticamente. È possibile verificare che sia stata creata, eseguire una ricerca nel *App\_dati* cartella. Se non viene visualizzato il *Movies.mdf* file, fare clic sul **Mostra tutti i file** pulsante il **Esplora soluzioni** sulla barra degli strumenti, fare clic sul **Aggiorna** pulsante e quindi espandere la *App\_dati* cartella.

![](accessing-your-models-data-from-a-controller/_static/image9.png)

Fare doppio clic su *Movies.mdf* per aprire **Esplora SERVER**, quindi espandere il **tabelle** cartella per visualizzare la tabella di film. Si noti l'icona della chiave accanto a ID. Per impostazione predefinita, Entity Framework userà una proprietà denominata ID della chiave primaria. Per altre informazioni su Entity Framework e MVC, vedere ottima esercitazione di Tom Dykstra sul [MVC ed Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")

Fare doppio clic il `Movies` tabella e selezionare **Mostra dati tabella** per visualizzare i dati è stato creato.

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

Fare doppio clic il `Movies` tabella e selezionare **Apri definizione tabella** per vedere la tabella di struttura che Entity Framework Code First creato automaticamente.

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

Si noti come lo schema del `Movies` esegue il mapping alla tabella il `Movie` classe creata in precedenza. Code First di Entity Framework creato automaticamente questo schema di base di `Movie` classe.

Al termine, chiudere la connessione facendo clic con il pulsante destro *MovieDBContext* e selezionando **Chiudi connessione**. (Se si non chiude la connessione, è possibile ottenere un errore alla successiva che esecuzione del progetto).

![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")

È ora disponibile un database e alcune pagine per visualizzare, modificare, aggiornare ed eliminare dati. Nella prossima esercitazione, verrà esaminare il resto del codice con scaffolding e aggiunta una `SearchIndex` metodo e un `SearchIndex` vista che consente di eseguire la ricerca di film nel database. Per altre informazioni sull'uso di Entity Framework con MVC, vedere [creazione di un modello di dati di Entity Framework per un'applicazione ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

> [!div class="step-by-step"]
> [Precedente](creating-a-connection-string.md)
> [Successivo](examining-the-edit-methods-and-edit-view.md)
