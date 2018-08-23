---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/accessing-your-models-data-from-a-controller
title: Accesso ai dati del modello da un Controller (c#) | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione insegnerà le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 002ada5c-f114-47ab-a441-57dbdb728ea0
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 6e517538f6ebb2c42f7c1d66efed7831c2c877fe
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835579"
---
<a name="accessing-your-models-data-from-a-controller-c"></a>Accesso ai dati del modello da un Controller (c#)
====================
da [Rick Anderson](https://github.com/Rick-Anderson)

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

In questa sezione si creerà un nuovo `MoviesController` classe e scrivere il codice che recupera i dati dei film e lo visualizza nel browser usando un modello di vista. Assicurarsi di compilare l'applicazione prima di procedere.

Fare doppio clic il *controller* cartella e creare un nuovo `MoviesController` controller. Selezionare le opzioni seguenti:

- Nome del controller: **MoviesController**. (Questo è il valore predefinito. )
- Modello: **Controller con azioni di lettura/scrittura e visualizzazioni, che usa Entity Framework**.
- Classe modello: **Movie (mvcmovie. Models)**.
- Classe del contesto dati: **MovieDBContext (mvcmovie. Models)**.
- Visualizzazioni: **Razor (CSHTML)**. (Predefinito).

[![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image2.png "AddScaffoldedMovieController")](accessing-your-models-data-from-a-controller/_static/image1.png)

Fare clic su **Aggiungi**. Visual Web Developer crea i file e cartelle seguenti:

- *Un MoviesController.cs* file di progetto *controller* cartella.
- Oggetto *film* cartella del progetto *viste* cartella.
- *Create. cshtml, cshtml, Details. cshtml, Edit. cshtml*, e *index. cshtml* nella nuova *viste\filmati* cartella.

[![NewMovieControllerScreenShot](accessing-your-models-data-from-a-controller/_static/image4.png "NewMovieControllerScreenShot")](accessing-your-models-data-from-a-controller/_static/image3.png)

Il meccanismo di scaffolding di ASP.NET MVC 3 creato automaticamente il CRUD (creare, leggere, aggiornare ed eliminare) i metodi di azione e visualizzazioni per l'utente. Ora disponibile un'applicazione web completamente funzionali che consente di creare, elencare, modificare ed eliminare le voci di filmato.

Eseguire l'applicazione e passare al `Movies` controller aggiungendo */Movies* all'URL nella barra degli indirizzi del browser. Poiché l'applicazione si basa il routing predefinito (definito nel *Global. asax* file), la richiesta del browser `http://localhost:xxxxx/Movies` viene indirizzato sul valore predefinito `Index` metodo di azione del `Movies` controller. In altre parole, la richiesta del browser `http://localhost:xxxxx/Movies` corrisponde effettivamente alla richiesta del browser `http://localhost:xxxxx/Movies/Index`. Il risultato è un elenco vuoto di film, perché ancora stato aggiunto uno.

![](accessing-your-models-data-from-a-controller/_static/image5.png)

### <a name="creating-a-movie"></a>Creazione di un film

Selezionare il collegamento **Crea nuovo**. Immettere alcuni dettagli sui filmati, quindi scegliere il **Create** pulsante.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Facendo clic sui **Create** pulsante fa sì che il modulo viene inviato al server, in cui le informazioni sui film viene salvati nel database. Si verrà quindi reindirizzati per il */Movies* URL, in cui è possibile visualizzare i film appena creato nell'elenco.

[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png "IndexWhenHarryMet")](accessing-your-models-data-from-a-controller/_static/image7.png)

Creare altre due voci di filmato. Provare i collegamenti **Modifica**, **Dettagli** e **Elimina**, che sono tutti funzionali.

## <a name="examining-the-generated-code"></a>Analisi del codice generato

Aprire il *Controllers\MoviesController.cs* del file ed esaminare il generato `Index` (metodo). Una parte del controller di film con la `Index` metodo è illustrato di seguito.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

La riga seguente dal `MoviesController` classe crea un'istanza di un contesto di database di film, come descritto in precedenza. È possibile utilizzare il contesto di database di film per eseguire una query, modificare ed eliminare i film.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Una richiesta per il `Movies` controller restituisce tutte le voci la `Movies` tabella del database di film e quindi passa i risultati per il `Index` visualizzazione.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modelli fortemente tipizzati e @model (parola chiave)

In precedenza in questa esercitazione si è visto come un controller può passare oggetti o dati a un modello di visualizzazione usando la `ViewBag` oggetto. Il `ViewBag` è un oggetto dinamico che fornisce un modo pratico con associazione tardiva per passare informazioni a una vista.

ASP.NET MVC fornisce inoltre la possibilità di passare fortemente tipizzata di oggetti per un modello di vista o dati. Questo approccio consente una migliore controllo fase di compilazione del codice ed esecuzione IntelliSense più completa nell'editor di Visual Web Developer è fortemente tipizzato. Usiamo questo approccio con il `MoviesController` classe e *index. cshtml* modello di visualizzazione.

Si noti che il codice crea un [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) dell'oggetto quando viene chiamato il `View` metodo helper nel `Index` metodo di azione. Il codice passa quindi questo `Movies` elenco dal controller alla vista:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

Includendo un `@model` informativa nella parte superiore del file di modello della vista, è possibile specificare il tipo di oggetto previsto dalla vista. Quando si crea il controller di film, Visual Web Developer include automaticamente quanto segue `@model` istruzione in cima il *index. cshtml* file:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

Ciò `@model` direttiva consente di accedere all'elenco di film che il controller ha passato alla vista usando un `Model` oggetto fortemente tipizzato. Ad esempio, nelle *index. cshtml* modello, il codice scorre i film in questo modo un `foreach` istruzione sull'oggetto fortemente tipizzato `Model` oggetto:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml)]

Poiché il `Model` oggetto è fortemente tipizzato (come un `IEnumerable<Movie>` oggetto), ognuno `item` oggetto nel ciclo viene tipizzato come `Movie`. Tra gli altri vantaggi, ciò significa che si ottiene controllo in fase di compilazione del codice e supporto di IntelliSense nell'editor di codice completo:

[![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image10.png "ModelIntellisene")](accessing-your-models-data-from-a-controller/_static/image9.png)

## <a name="working-with-sql-server-compact"></a>Utilizzo di SQL Server Compact

Code First di Entity Framework ha rilevato che la stringa di connessione di database che è stata fornita puntata un `Movies` database che non esiste ancora, in modo da Code First ha creato il database automaticamente. È possibile verificare che sia stata creata, eseguire una ricerca nel *App\_dati* cartella. Se non viene visualizzato il *Movies.sdf* file, fare clic sul **Mostra tutti i file** pulsante il **Esplora soluzioni** sulla barra degli strumenti, fare clic sul **Aggiorna** pulsante e quindi espandere la *App\_dati* cartella.

[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png "SDF_in_SolnExp")](accessing-your-models-data-from-a-controller/_static/image11.png)

Fare doppio clic su *Movies.sdf* per aprire **Esplora Server**. Espandere quindi il **tabelle** cartella per visualizzare le tabelle che sono state create nel database.

> [!NOTE]
> Se si verifica un errore quando si fa doppio clic *Movies.sdf*, assicurarsi di aver installato [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime e strumenti di supportano). (Per i collegamenti al software, vedere l'elenco dei prerequisiti nella parte 1 di questa serie di esercitazioni). Se si installa la versione a questo punto, è necessario chiudere e riaprire Visual Web Developer.


[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png "DB_explorer")](accessing-your-models-data-from-a-controller/_static/image13.png)

Sono presenti due tabelle, una per il `Movie` set di entità e quindi il `EdmMetadata` tabella. Il `EdmMetadata` tabella viene utilizzata da Entity Framework per determinare quando il modello e il database non sono sincronizzati.

Fare doppio clic il `Movies` tabella e selezionare **Mostra dati tabella** per visualizzare i dati è stato creato.

[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png "MoviesTable")](accessing-your-models-data-from-a-controller/_static/image15.png)

Fare doppio clic il `Movies` tabella e selezionare **modificare lo Schema di tabella**.

[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png "EditTableSchema")](accessing-your-models-data-from-a-controller/_static/image17.png)

[![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image20.png "TableSchemaSM")](accessing-your-models-data-from-a-controller/_static/image19.png)

Si noti come lo schema del `Movies` esegue il mapping alla tabella il `Movie` classe creata in precedenza. Code First di Entity Framework creato automaticamente questo schema di base di `Movie` classe.

Al termine, chiudere la connessione. (Se si non chiude la connessione, è possibile ottenere un errore alla successiva che esecuzione del progetto).

[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image22.png "CloseConnection")](accessing-your-models-data-from-a-controller/_static/image21.png)

È ora disponibile il database e una pagina di inserzione semplice per visualizzare il contenuto da quest'ultimo. Nella prossima esercitazione, verrà esaminare il resto del codice con scaffolding e aggiunta una `SearchIndex` metodo e un `SearchIndex` vista che consente di eseguire la ricerca di film nel database.

> [!div class="step-by-step"]
> [Precedente](adding-a-model.md)
> [Successivo](examining-the-edit-methods-and-edit-view.md)
