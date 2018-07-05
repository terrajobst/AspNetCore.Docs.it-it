---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: Accesso ai dati del modello da un Controller | Microsoft Docs
author: Rick-Anderson
description: 'Nota: Una versione aggiornata di questa esercitazione è disponibile qui che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro e molto più semplice da seguire e demo...'
ms.author: aspnetcontent
ms.date: 08/28/2012
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: cf1d27088c1e65d55a6820825eebe63f7fdcb515
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804937"
---
<a name="accessing-your-models-data-from-a-controller"></a>Accesso ai dati del modello da un Controller
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > È disponibile una versione aggiornata di questa esercitazione [qui](../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e vengono illustrate altre funzionalità.


In questa sezione si creerà un nuovo `MoviesController` classe e scrivere il codice che recupera i dati dei film e lo visualizza nel browser usando un modello di vista.

**Compilare l'applicazione** prima di procedere al passaggio successivo.

Fare doppio clic il *controller* cartella e creare un nuovo `MoviesController` controller. Le opzioni seguenti non verranno visualizzati fino a quando non si compila l'applicazione. Selezionare le opzioni seguenti:

- Nome del controller: **MoviesController**. (Questo è il valore predefinito. )
- Modello: **Controller MVC con azioni di lettura/scrittura e visualizzazioni, mediante Entity Framework**.
- Classe modello: **Movie (mvcmovie. Models)**.
- Classe del contesto dati: **MovieDBContext (mvcmovie. Models)**.
- Visualizzazioni: **Razor (CSHTML)**. (Predefinito).

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

Fare clic su **Aggiungi**. Visual Studio Express consente di creare i file e cartelle seguenti:

- *Un MoviesController.cs* file di progetto *controller* cartella.
- Oggetto *film* cartella del progetto *viste* cartella.
- *Create. cshtml, cshtml, Details. cshtml, Edit. cshtml*, e *index. cshtml* nella nuova *viste\filmati* cartella.

ASP.NET MVC 4 creato automaticamente il CRUD (creare, leggere, aggiornare ed eliminare) i metodi di azione e visualizzazioni per l'utente (la creazione automatica di metodi di azione CRUD e le visualizzazioni è nota come scaffolding). Ora disponibile un'applicazione web completamente funzionali che consente di creare, elencare, modificare ed eliminare le voci di filmato.

Eseguire l'applicazione e passare al `Movies` controller aggiungendo */Movies* all'URL nella barra degli indirizzi del browser. Poiché l'applicazione si basa il routing predefinito (definito nel *Global. asax* file), la richiesta del browser `http://localhost:xxxxx/Movies` viene indirizzato sul valore predefinito `Index` metodo di azione del `Movies` controller. In altre parole, la richiesta del browser `http://localhost:xxxxx/Movies` corrisponde effettivamente alla richiesta del browser `http://localhost:xxxxx/Movies/Index`. Il risultato è un elenco vuoto di film, perché ancora stato aggiunto uno.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a>Creazione di un film

Selezionare il collegamento **Crea nuovo**. Immettere alcuni dettagli sui filmati, quindi scegliere il **Create** pulsante.

![](accessing-your-models-data-from-a-controller/_static/image3.png)

Facendo clic sui **Create** pulsante fa sì che il modulo viene inviato al server, in cui le informazioni sui film viene salvati nel database. Si verrà quindi reindirizzati per il */Movies* URL, in cui è possibile visualizzare i film appena creato nell'elenco.

![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")

Creare altre due voci di filmato. Provare i collegamenti **Modifica**, **Dettagli** e **Elimina**, che sono tutti funzionali.

## <a name="examining-the-generated-code"></a>Analisi del codice generato

Aprire il *Controllers\MoviesController.cs* del file ed esaminare il generato `Index` (metodo). Una parte del controller di film con la `Index` metodo è illustrato di seguito.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

La riga seguente dal `MoviesController` classe crea un'istanza di un contesto di database di film, come descritto in precedenza. È possibile utilizzare il contesto di database di film per eseguire una query, modificare ed eliminare i film.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Una richiesta per il `Movies` controller restituisce tutte le voci la `Movies` tabella del database di film e quindi passa i risultati per il `Index` visualizzazione.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modelli fortemente tipizzati e @model (parola chiave)

In precedenza in questa esercitazione si è visto come un controller può passare oggetti o dati a un modello di visualizzazione usando la `ViewBag` oggetto. Il `ViewBag` è un oggetto dinamico che fornisce un modo pratico con associazione tardiva per passare informazioni a una vista.

ASP.NET MVC fornisce inoltre la possibilità di passare fortemente tipizzata di oggetti per un modello di vista o dati. Questo approccio consente una migliore controllo fase di compilazione del codice ed esecuzione IntelliSense più completa nell'editor di Visual Studio è fortemente tipizzato. Il meccanismo di scaffolding in Visual Studio usato questo approccio con il `MoviesController` i modelli di classe e visualizzazione della creazione di metodi e visualizzazioni.

Nel *Controllers\MoviesController.cs* file esaminare generato `Details` (metodo). Una parte del controller di film con la `Details` metodo è illustrato di seguito.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

Se un `Movie` viene trovato, un'istanza di `Movie` modello viene passato alla visualizzazione dettagli. Esaminare il contenuto del *Views\Movies\Details.cshtml* file.

Includendo un `@model` informativa nella parte superiore del file di modello della vista, è possibile specificare il tipo di oggetto previsto dalla vista. Al momento della creazione del controller di film, l'istruzione `@model` è stata inclusa automaticamente in Visual Studio all'inizio del file *Details.cshtml*:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

Questa direttiva `@model` consente di accedere al film passato dal controller alla vista usando un oggetto `Model` fortemente tipizzato. Ad esempio, nelle *Details. cshtml* modello, il codice passa ogni campo di film per il `DisplayNameFor` e [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) helper HTML con l'oggetto fortemente tipizzato `Model` oggetto. I metodi di creazione e modifica e i modelli di vista anche passano un oggetto modello di film.

Esaminare i *index. cshtml* modello di visualizzazione e il `Index` metodo nel *MoviesController.cs* file. Si noti che il codice crea un [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) dell'oggetto quando viene chiamato il `View` metodo helper nel `Index` metodo di azione. Il codice passa quindi questo `Movies` elenco dal controller alla vista:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

Quando è stato creato il controller di film, Visual Studio Express viene incluso automaticamente quanto segue `@model` istruzione in cima il *index. cshtml* file:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Ciò `@model` direttiva consente di accedere all'elenco di film che il controller ha passato alla vista usando un `Model` oggetto fortemente tipizzato. Ad esempio, nelle *index. cshtml* modello, il codice scorre i film in questo modo un `foreach` istruzione sull'oggetto fortemente tipizzato `Model` oggetto:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

Poiché il `Model` oggetto è fortemente tipizzato (come un `IEnumerable<Movie>` oggetto), ognuno `item` oggetto nel ciclo viene tipizzato come `Movie`. Tra gli altri vantaggi, ciò significa che si ottiene controllo in fase di compilazione del codice e supporto di IntelliSense nell'editor di codice completo:

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a>Utilizzo di SQL Server Local DB

Code First di Entity Framework ha rilevato che la stringa di connessione di database che è stata fornita puntata un `Movies` database che non esiste ancora, in modo da Code First ha creato il database automaticamente. È possibile verificare che sia stata creata, eseguire una ricerca nel *App\_dati* cartella. Se non viene visualizzato il *Movies.mdf* file, fare clic sul **Mostra tutti i file** pulsante il **Esplora soluzioni** sulla barra degli strumenti, fare clic sul **Aggiorna** pulsante e quindi espandere la *App\_dati* cartella.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Fare doppio clic su *Movies.mdf* per aprire **Esplora DATABASE**, quindi espandere il **tabelle** cartella per visualizzare la tabella di film.

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")

> [!NOTE]
> Se non viene visualizzato Esplora database, dal **degli strumenti** dal menu **Connetti al Database**, quindi annullare la **Scegli origine dati** finestra di dialogo. Questa operazione forzerà aperto database explorer.


> [!NOTE]
> Se si usa VWD o Visual Studio 2010 e si verifica un errore simile a uno dei seguenti seguenti:
> 
> - Il database ' C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES. File MDF' non può essere aperto perché la versione 706 è. Questo server supporta 655 e nelle versioni precedenti. Un percorso per il downgrade non è supportato.
> - &quot;Non è stata gestita dal codice utente eccezioni InvalidOperation&quot; nella stringa SqlConnection fornita non specifica un catalogo iniziale.
> 
> È necessario installare il [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) e [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0). Verificare il `MovieDBContext` stringa di connessione specificata nella pagina precedente.


Fare doppio clic il `Movies` tabella e selezionare **Mostra dati tabella** per visualizzare i dati è stato creato.

![](accessing-your-models-data-from-a-controller/_static/image8.png)

Fare doppio clic il `Movies` tabella e selezionare **Apri definizione tabella** per vedere la tabella di struttura che Entity Framework Code First creato automaticamente.

![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")

![](accessing-your-models-data-from-a-controller/_static/image10.png)

Si noti come lo schema del `Movies` esegue il mapping alla tabella il `Movie` classe creata in precedenza. Code First di Entity Framework creato automaticamente questo schema di base di `Movie` classe.

Al termine, chiudere la connessione facendo clic con il pulsante destro *MovieDBContext* e selezionando **Chiudi connessione**. (Se si non chiude la connessione, è possibile ottenere un errore alla successiva che esecuzione del progetto).

![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")

È ora disponibile il database e una pagina di inserzione semplice per visualizzare il contenuto da quest'ultimo. Nella prossima esercitazione, verrà esaminare il resto del codice con scaffolding e aggiunta una `SearchIndex` metodo e un `SearchIndex` vista che consente di eseguire la ricerca di film nel database.

> [!div class="step-by-step"]
> [Precedente](adding-a-model.md)
> [Successivo](examining-the-edit-methods-and-edit-view.md)
