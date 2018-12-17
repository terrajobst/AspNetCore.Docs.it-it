---
title: Introduzione all'uso di pagine Razor in ASP.NET Core
author: rick-anderson
monikerRange: '>= aspnetcore-2.2'
description: Questa serie di esercitazioni illustra come usare Razor Pages in ASP.NET Core. Offre informazioni su come creare un modello, generare codice per Razor Pages, usare Entity Framework Core e SQL Server per l'accesso ai dati, aggiungere funzionalità di ricerca, aggiungere la convalida dell'input e usare le migrazioni per aggiornare il modello.
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 1152ebfcee48a46ecd28c941fce32d3fc1e05c41
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861628"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a>Esercitazione: Introduzione all'uso di Razor Pages in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Questa esercitazione illustra le nozioni di base della creazione di un'app Web pagine Razor di ASP.NET Core.

L'app gestisce un database di titoli di film. Vengono illustrate le seguenti procedure:

> [!div class="checklist"]
> * Creare un'app Web di Razor Pages.
> * Aggiungere un modello ed eseguirne lo scaffolding.
> * Usare un database.
> * Aggiungere ricerca e convalida.

Al termine di queste operazioni si ottiene un'app che può gestire e visualizzare gli elementi costituiti da titoli di film.

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-web-app"></a>Creare un'app Web Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.
* Creare una nuova applicazione Web ASP.NET Core. Denominare il progetto **RazorPagesMovie**. È importante denominare il progetto *RazorPagesMovie* in modo che gli spazi dei nomi corrispondano nell'operazione di copia/incolla del codice.
 ![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* Selezionare **ASP.NET Core 2.2** nell'elenco a discesa e quindi selezionare **Applicazione Web**.

  ![nuova applicazione Web ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  Viene creato il progetto iniziale seguente:

  ![Esplora soluzioni](razor-pages-start/_static/se2.2.png)

* Premere **CTRL+F5** per l'esecuzione senza il debugger.

  Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app. La barra degli indirizzi visualizza `localhost:port#` e non `example.com` o simili. Ciò accade perché `localhost` è il nome host standard per il computer locale. Localhost viene usato solo per le richieste web del computer locale. Quando Visual Studio crea un progetto Web, per il server Web viene usata una porta casuale. Nell'immagine precedente, il numero di porta è 5001. Quando si esegue l'app verrà visualizzato un numero di porta diverso.

  Se si avvia l'app con **CTRL+F5** (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice. Molti sviluppatori preferiscono usare la modalità non di debug per aggiornare la pagina e visualizzare le modifiche.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Cambiare directory (`cd`) e passare alla cartella che conterrà il progetto.
* Eseguire il comando seguente:

   ```console
   dotnet new webapp -o RazorPagesMovie
   code -r RazorPagesMovie
   ```

  * Viene visualizzata una finestra di dialogo con **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?** (Risorse di compilazione e debug mancanti da "RazorPagesMovie". Aggiungerle?)  Selezionare **Sì**.

  * `dotnet new webapp -o RazorPagesMovie`: crea un nuovo progetto Razor Pages nella cartella *RazorPagesMovie*.
  * `code -r RazorPagesMovie`: carica il file di progetto *RazorPagesMovie.csproj* in Visual Studio Code.

### <a name="launch-the-app"></a>Avviare l'app

* Premere **CTRL+F5** per l'esecuzione senza il debugger.

  Visual Studio Code avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5001`. La barra degli indirizzi visualizza `localhost:port:5001` e non `example.com` o simili. Ciò accade perché `localhost` è il nome host standard per il computer locale. Localhost viene usato solo per le richieste web del computer locale.

  Se si avvia l'app con **CTRL+F5** (modalità non di debug) sarà possibile apportare modifiche al codice, salvare il file, aggiornare il browser e visualizzare le modifiche del codice. Molti sviluppatori preferiscono usare la modalità non di debug per aggiornare la pagina e visualizzare le modifiche.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

Da un terminale eseguire i comandi seguenti:

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

I comandi precedenti usano [.NET Core CLI](/dotnet/core/tools/dotnet) per creare ed eseguire un progetto di pagine Razor. Aprire un browser all'indirizzo http://localhost:5000 per visualizzare l'applicazione.

## <a name="open-the-project"></a>Aprire il progetto

Premere Ctrl + C per arrestare l'applicazione.

Da Visual Studio, selezionare **File > Apri**, quindi selezionare il file *RazorPagesMovie.csproj*.

### <a name="launch-the-app"></a>Avviare l'app

Per avviare l'app, selezionare **Esegui > Avvia senza eseguire debug**. Visual Studio avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5001`.

<!-- End of VS tabs -->

---

* Selezionare **Accept** (Accetto) per autorizzare il rilevamento. Questa app non rileva informazioni personali. Il codice generato del modello include asset che consentono di soddisfare il [Regolamento generale sulla protezione dei dati (GDPR)](xref:security/gdpr).

  ![Pagina Home o di indice](razor-pages-start/_static/homeGDPR2.2.png)

  La figura seguente illustra l'app dopo aver accettato il rilevamento:

  ![Pagina Home o di indice](razor-pages-start/_static/home2.2.png)

## <a name="project-files-and-folders"></a>File e cartelle di progetto

La tabella seguente elenca i file e le cartelle nel progetto. A questo punto dell'esercitazione, il file più importante da comprendere è *Startup.cs*. Non è necessario rivedere ogni collegamento indicato di seguito. I collegamenti sono forniti come riferimento quando sono necessarie altre informazioni su un file o una cartella nel progetto.

| File o cartella              | Scopo |
| ----------------- | ------------ |
| *wwwroot* | Contiene file statici. Vedere [File statici](xref:fundamentals/static-files). |
| *Pagine* | Cartella per [Pagine Razor](xref:razor-pages/index). |
| *appsettings.json* | [Configurazione](xref:fundamentals/configuration/index) |
| *Program.cs* | [Ospita](xref:fundamentals/host/index) l'app ASP.NET Core.|
| *Startup.cs* | Configura i servizi e la pipeline della richiesta. Vedere [Avvio](xref:fundamentals/startup).|

### <a name="the-pages-folder"></a>Cartella delle pagine

Il file *_Layout.cshtml* contiene elementi HTML comuni (script e fogli di stile) e imposta il layout per l'applicazione. Ad esempio, facendo clic su **RazorPagesMovie**, **Home**, o **Privacy**, vengono visualizzati gli stessi elementi. Gli elementi comuni includono il menu di navigazione nella parte superiore e l'intestazione nella parte inferiore della finestra. Vedere [Layout](xref:mvc/views/layout) per altre informazioni.

Il file *_ViewImports.cshtml* contiene le direttive Razor che vengono importate in ogni pagina Razor. Per altre informazioni, vedere [Importazione di direttive condivise](xref:mvc/views/layout#importing-shared-directives).

*_ViewStart.cshtml* imposta la proprietà pagine Razor `Layout` per l'uso del file *_Layout.cshtml*. Vedere [Layout](xref:mvc/views/layout) per altre informazioni.

Il file *_ValidationScriptsPartial.cshtml* fornisce un riferimento agli script di convalida [jQuery](https://jquery.com/). Quando si aggiungono le pagine `Create` e `Edit` in un secondo momento nell'esercitazione, viene usato il file *_ValidationScriptsPartial.cshtml*.

Le pagine `Index`, `Error`, e `Privacy` vengono specificate per le operazioni seguenti:

* `Index`: avvia un'app.
* `Error`: visualizza informazioni sull'errore.
* `Privacy`: specifica i dettagli dell'informativa sulla privacy del sito.

Per questa esercitazione, queste pagine non vengono usate.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

<a name="f7"></a>
### <a name="use-f7-to-toggle-between-a-razor-page-and-the-pagemodel"></a>Premere F7 per spostarsi tra una pagina Razor Pages e il PageModel

Per passare da una pagina Razor (file con estensione *\*cshtml*) al file C# (con estensione *\*cshtml.cs*), premere F7.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!-- TODO review  Need something in these tabs -->

Per convenzione, la pagina Razor (file con estensione *\*cshtml*) e l'elemento `PageModel` associato hanno lo stesso nome file radice.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

Per convenzione, la pagina Razor (file con estensione *\*cshtml*) e l'elemento `PageModel` associato hanno lo stesso nome file radice.

---

> [!div class="step-by-step"]
> [Avanti: Aggiunta di un modello](xref:tutorials/razor-pages/model)