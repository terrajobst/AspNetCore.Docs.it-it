---
title: Eseguire la migrazione da ASP.NET MVC ad ASP.NET Core MVC
author: ardalis
description: Informazioni su come iniziare a usare la migrazione di un progetto ASP.NET MVC ad ASP.NET Core MVC.
ms.author: riande
ms.date: 04/06/2019
uid: migration/mvc
ms.openlocfilehash: a85b9f15be8ad9ca66b20ef1f4422fe67806a797
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/10/2019
ms.locfileid: "59468540"
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a>Eseguire la migrazione da ASP.NET MVC ad ASP.NET Core MVC

Dal [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), e [Scott Addie](https://scottaddie.com)

Questo articolo illustra come iniziare a usare la migrazione di un progetto ASP.NET MVC [ASP.NET Core MVC](../mvc/overview.md). Nel processo, verrà evidenziato gran parte delle operazioni che sono stati modificati da ASP.NET MVC. La migrazione da ASP.NET MVC è un processo in più passaggi e questo articolo illustra la configurazione iniziale, i controller di base e viste, contenuto statico e le dipendenze dal lato client. Articoli aggiuntivi illustrano migrazione della configurazione e il codice di identità disponibili in molti progetti ASP.NET MVC.

> [!NOTE]
> I numeri di versione indicato negli esempi potrebbero non essere aggiornati. Si potrebbe essere necessario aggiornare di conseguenza i progetti.

## <a name="create-the-starter-aspnet-mvc-project"></a>Creare il progetto ASP.NET MVC di base

Per illustrare l'aggiornamento, si inizierà creando un'app ASP.NET MVC. Crearlo con il nome *App Web 1* in modo che lo spazio dei nomi corrisponde al progetto di ASP.NET Core verrà creata nel passaggio successivo.

![Finestra di dialogo di Visual Studio nuovo progetto](mvc/_static/new-project.png)

![Nuova finestra di dialogo dell'applicazione Web: Modello di progetto MVC selezionato nel Pannello di modelli ASP.NET](mvc/_static/new-project-select-mvc-template.png)

*Facoltativo:* Modificare il nome della soluzione da *App Web 1* al *Mvc5*. Visual Studio visualizza il nuovo nome della soluzione (*Mvc5*), che rende più semplice indicare il progetto dal progetto successivo.

## <a name="create-the-aspnet-core-project"></a>Creare il progetto ASP.NET Core

Creare una nuova *vuote* app web ASP.NET Core con lo stesso nome del progetto precedente (*App Web 1*) in modo che corrispondano agli spazi dei nomi dei due progetti. Lo stesso spazio dei nomi rende più semplice copiare codice tra i due progetti. È possibile creare il progetto in una directory diversa da quella del progetto precedente per usare lo stesso nome.

![Finestra di dialogo Nuovo progetto](mvc/_static/new_core.png)

![Nuova finestra di dialogo dell'applicazione Web ASP.NET: Modello di progetto vuoto selezionato nel Pannello di modelli di ASP.NET Core](mvc/_static/new-project-select-empty-aspnet5-template.png)

* *Facoltativo:* Creare una nuova app ASP.NET Core usando il *applicazione Web* modello di progetto. Denominare il progetto *App Web 1*, quindi selezionare un'opzione di autenticazione di **singoli account utente di**. Rinomina l'app per *FullAspNetCore*. Creazione di questo consente di risparmiare progetto tempo durante la conversione. È possibile esaminare il codice modello generato per visualizzare il risultato finale o copiare codice al progetto di conversione. È inoltre utile quando bloccarsi su un'istruzione di conversione da confrontare con il progetto di modello generato.

## <a name="configure-the-site-to-use-mvc"></a>Configurare il sito per l'uso di MVC

::: moniker range=">= aspnetcore-2.1"

* Quando la destinazione è .NET Core, il [metapacchetto Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) viene fatto riferimento per impostazione predefinita. Questo pacchetto contiene i pacchetti di pacchetti comunemente usati dalle App MVC. Se la destinazione è .NET Framework, riferimenti ai pacchetti deve essere elencate singolarmente nel file di progetto.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* Quando la destinazione è .NET Core, il [metapacchetto Microsoft. aspnetcore](xref:fundamentals/metapackage) viene fatto riferimento per impostazione predefinita. Questo pacchetto contiene i pacchetti di pacchetti comunemente usati dalle App MVC. Se la destinazione è .NET Framework, riferimenti ai pacchetti deve essere elencate singolarmente nel file di progetto.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* Quando la destinazione è .NET Core o .NET Framework, i pacchetti di pacchetti comunemente usati dalle App MVC sono elencati singolarmente nel file di progetto.

::: moniker-end

`Microsoft.AspNetCore.Mvc` è il framework di ASP.NET Core MVC. `Microsoft.AspNetCore.StaticFiles` è il gestore di file statici. Il runtime di ASP.NET Core è modulare e deve esplicitamente acconsentire esplicitamente a usare i file statici (vedere [i file statici](xref:fundamentals/static-files)).

* Aprire il *Startup.cs* di file e modificare il codice in base a quanto segue:

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

Il `UseStaticFiles` metodo di estensione aggiunge il gestore di file statici. Come accennato in precedenza, il runtime di ASP.NET è modulare e deve esplicitamente acconsentire esplicitamente a usare i file statici. Il `UseMvc` aggiunge il metodo di estensione routing. Per altre informazioni, vedere [avvio dell'applicazione](xref:fundamentals/startup) e [Routing](xref:fundamentals/routing).

## <a name="add-a-controller-and-view"></a>Aggiungere un controller e visualizzazione

In questa sezione si aggiungerà un controller di minima e una visualizzazione a essere utilizzati come segnaposti per i controller ASP.NET MVC e le viste che è possibile eseguire la migrazione nella sezione successiva.

* Aggiungere un *controller* cartella.

* Aggiungere un **classe Controller** denominate *HomeController.cs* per il *controller* cartella.

![Finestra di dialogo Aggiungi nuovo elemento](mvc/_static/add_mvc_ctl.png)

* Aggiungere un *viste* cartella.

* Aggiungere un *Views/Home* cartella.

* Aggiungere un **visualizzazione Razor** denominate *index. cshtml* per il *Views/Home* cartella.

![Finestra di dialogo Aggiungi nuovo elemento](mvc/_static/view.png)

La struttura del progetto è la seguente:

![Esplora soluzioni con file e cartelle dell'App Web 1](mvc/_static/project-structure-controller-view.png)

Sostituire il contenuto del *Views/Home/Index.cshtml* file con il codice seguente:

```html
<h1>Hello world!</h1>
```

Eseguire l'app.

![App Web aperta in Microsoft Edge](mvc/_static/hello-world.png)

Visualizzare [controller](xref:mvc/controllers/actions) e [viste](xref:mvc/views/overview) per altre informazioni.

Ora che abbiamo un progetto ASP.NET Core lavoro minimo, è possibile iniziare la migrazione delle funzionalità dal progetto ASP.NET MVC. Sarà necessario passare quanto segue:

* contenuto sul lato client (CSS, i tipi di carattere e gli script)

* controller

* visualizzazioni

* modelli

* Creazione di bundle

* filtri

* Il log in ingresso/uscita, Identity (ciò viene eseguito nella prossima esercitazione.)

## <a name="controllers-and-views"></a>Controller e visualizzazioni

* Ognuno dei metodi di copia da ASP.NET MVC `HomeController` alla nuova `HomeController`. Si noti che in ASP.NET MVC, tipo restituito del metodo del modello incorporato controller azione sia [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, i metodi di azione restituiti `IActionResult` invece. `ActionResult` implementa `IActionResult`, pertanto non è necessario modificare il tipo restituito dei metodi di azione.

* Copia il *About. cshtml*, *Contact. cshtml*, e *index. cshtml* Visualizza file dal progetto ASP.NET MVC Razor per il progetto ASP.NET Core.

* Eseguire l'app ASP.NET Core e ogni metodo di test. È ancora stato ancora eseguito la migrazione il file di layout o gli stili, in modo che le viste visualizzabili contengono solo il contenuto nei file di visualizzazione. Non sarà necessario i collegamenti generati file di layout per la `About` e `Contact` viste, pertanto è necessario invece richiamarle dal browser (sostituire **4492** con il numero di porta utilizzato nel progetto).

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Pagina contatto](mvc/_static/contact-page.png)

Si noti la mancanza di applicazione di stili e voci di menu. Questo problema verrà corretto nella prossima sezione.

## <a name="static-content"></a>Contenuto statico

Nelle versioni precedenti di ASP.NET MVC, contenuto statico è stato ospitato dalla radice del progetto web ed è stato combinato con un file lato server. In ASP.NET Core, il contenuto statico è ospitato nel *wwwroot* cartella. È opportuno copiare il contenuto statico dalla propria app ASP.NET MVC precedente per il *wwwroot* cartella nel progetto ASP.NET Core. In questa conversione di esempio:

* Copia il */favicon.ico* file dal progetto MVC precedente per il *wwwroot* cartella nel progetto ASP.NET Core.

ASP.NET MVC il vecchio progetto viene utilizzato [Bootstrap](https://getbootstrap.com/) per la relativa applicazione di stili e archivi di bootstrap relativo ai file nei *contenuto* e *script* cartelle. Il modello, che ha generato il vecchio progetto ASP.NET MVC, fa riferimento a Bootstrap nel file di layout (*Views/Shared/_Layout.cshtml*). È possibile copiare il *bootstrap. js* e *bootstrap* da MVC ASP.NET i file di progetto per il *wwwroot* cartella nel nuovo progetto. Al contrario, si aggiungerà il supporto per Bootstrap e altre librerie lato client tramite le reti CDN nella sezione successiva.

## <a name="migrate-the-layout-file"></a>Eseguire la migrazione di file di layout

* Copia il *viewstart. cshtml* file dal progetto ASP.NET MVC precedente *viste* nella cartella del progetto ASP.NET Core *viste* cartella. Il *viewstart. cshtml* file non è stato modificato in ASP.NET Core MVC.

* Creare un *Views/Shared* cartella.

* *Facoltativo:* Copia *viewimports. cshtml* dalle *FullAspNetCore* del progetto MVC *viste* nella cartella del progetto ASP.NET Core *viste* cartella. Rimuovere le dichiarazioni dello spazio dei nomi nella *viewimports. cshtml* file. Il *viewimports. cshtml* file fornisce gli spazi dei nomi per tutti i file di visualizzazione e riporta [gli helper Tag](xref:mvc/views/tag-helpers/intro). Gli helper tag vengono usati nel nuovo file di layout. Il *viewimports. cshtml* file è una novità di ASP.NET Core.

* Copia il *layout. cshtml* file dal progetto ASP.NET MVC precedente *Views/Shared* nella cartella del progetto ASP.NET Core *Views/Shared* cartella.

Aprire *layout. cshtml* file e apportare le modifiche seguenti (il codice completo è illustrato di seguito):

* Sostituire `@Styles.Render("~/Content/css")` con un `<link>` elemento caricare *bootstrap* (vedere sotto).

* Rimuovere `@Scripts.Render("~/bundles/modernizr")`.

* Impostare come commento il `@Html.Partial("_LoginPartial")` line (racchiudere la riga con `@*...*@`). Per altre informazioni vedere [eseguire la migrazione di autenticazione e identità ad ASP.NET Core](xref:migration/identity)

* Sostituire `@Scripts.Render("~/bundles/jquery")` con un `<script>` elemento (vedere sotto).

* Sostituire `@Scripts.Render("~/bundles/bootstrap")` con un `<script>` elemento (vedere sotto).

Il markup di sostituzione per l'inclusione di CSS Bootstrap:

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

Il markup di sostituzione per jQuery e JavaScript Bootstrap inclusione:

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

Aggiornato *layout. cshtml* file è illustrato di seguito:

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

Visualizzare il sito nel browser. A questo punto dovrebbero essere caricati correttamente, con gli stili previsto posto.

* *Facoltativo:* È possibile provare a usare il nuovo file di layout. Per questo progetto è possibile copiare il file di layout dal *FullAspNetCore* progetto. Usa il nuovo file di layout [helper Tag](xref:mvc/views/tag-helpers/intro) e sono stati apportati altri miglioramenti.

## <a name="configure-bundling-and-minification"></a>Configurare la creazione di bundle e minimizzazione

Per informazioni su come configurare la creazione di bundle e minimizzazione, vedere [Bundling and Minification](../client-side/bundling-and-minification.md).

## <a name="solve-http-500-errors"></a>Risolvere gli errori HTTP 500

Esistono molti problemi che possono causare un messaggio di errore HTTP 500 che non contengono informazioni sull'origine del problema. Ad esempio, se il *viewimports* file contiene uno spazio dei nomi che non esiste nel progetto, si otterrà un errore HTTP 500. Per impostazione predefinita nelle App ASP.NET Core, il `UseDeveloperExceptionPage` viene aggiunta l'estensione per il `IApplicationBuilder` eseguiti quando la configurazione è *sviluppo*. Ciò è descritta in dettaglio il codice seguente:

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

ASP.NET Core converte le eccezioni non gestite in un'app web nelle risposte di errore HTTP 500. In genere, i dettagli dell'errore non sono inclusi in queste risposte per impedire la divulgazione di informazioni potenzialmente riservate sul server. Vedere **usando la pagina delle eccezioni per gli sviluppatori** nelle [gestione degli errori](../fundamentals/error-handling.md) per altre informazioni.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:razor-components/index>
* <xref:mvc/views/tag-helpers/intro>
