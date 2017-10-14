---
title: Migrazione da MVC ASP.NET ad ASP.NET MVC di base
author: ardalis
description: 
keywords: ASP.NET Core, MVC, migrazione
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: article
ms.assetid: 3155cc9e-d0c9-424b-886c-35c0ec6f9f4e
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/mvc
ms.openlocfilehash: 7a4357da4cc97d7c60cc7e309add7583ef096597
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/13/2017
---
# <a name="migrating-from-aspnet-mvc-to-aspnet-core-mvc"></a>Migrazione da MVC ASP.NET ad ASP.NET MVC di base

Da [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), e [Scott Addie](https://scottaddie.com)

Questo articolo illustra come iniziare la migrazione di un progetto ASP.NET MVC per [ASP.NET MVC Core](../mvc/overview.md). Nel processo, viene illustrata la gran parte delle operazioni che sono stati modificati da ASP.NET MVC. La migrazione da ASP.NET MVC è un processo in più passaggi e questo articolo vengono illustrate la configurazione iniziale, i controller di base e viste, contenuto statico e dipendenze sul lato client. Articoli aggiuntivi coprono la migrazione della configurazione e il codice di identità presente in molti progetti ASP.NET MVC.

> [!NOTE]
> I numeri di versione campioni potrebbero non essere aggiornati. Si potrebbe essere necessario aggiornare di conseguenza i progetti.

## <a name="create-the-starter-aspnet-mvc-project"></a>Creare il progetto ASP.NET MVC starter

Per dimostrare l'aggiornamento, si inizierà creando un'applicazione ASP.NET MVC. Crearla con il nome *WebApp1* in modo lo spazio dei nomi corrisponderà il progetto ASP.NET Core creata nel passaggio successivo.

![Finestra di dialogo di Visual Studio nuovo progetto](mvc/_static/new-project.png)

![Finestra di dialogo nuova applicazione Web: il modello di progetto MVC selezionato nel riquadro modelli ASP.NET](mvc/_static/new-project-select-mvc-template.png)

*Facoltativo:* modificare il nome della soluzione da *WebApp1* a *Mvc5*. Verrà visualizzato il nome della nuova soluzione in Visual Studio (*Mvc5*), che renderà più facile indicare il progetto dal progetto successivo.

## <a name="create-the-aspnet-core-project"></a>Creare il progetto ASP.NET Core

Creare un nuovo *vuoto* app web ASP.NET Core con lo stesso nome del progetto precedente (*WebApp1*) in modo da corrispondere gli spazi dei nomi in due progetti. Lo stesso spazio dei nomi rende più semplice copiare il codice tra i due progetti. È necessario creare il progetto in una directory diversa da quella del progetto precedente per utilizzare lo stesso nome.

![Finestra di dialogo Nuovo progetto](mvc/_static/new_core.png)

![Finestra di dialogo nuova applicazione Web ASP.NET: modello di progetto vuoto selezionato nel Pannello di ASP.NET Core modelli](mvc/_static/new-project-select-empty-aspnet5-template.png)

* *Facoltativo:* crea una nuova applicazione ASP.NET Core usando il *applicazione Web* modello di progetto. Denominare il progetto *WebApp1*, quindi selezionare un'opzione di autenticazione di **singoli account utente di**. Rinominare questa app e *FullAspNetCore*. Creazione di questo progetto verranno risparmiare tempo durante la conversione. È possibile esaminare il codice modello generato per visualizzare il risultato finale o copiare il codice per il progetto di conversione. È inoltre utile quando è bloccato nel passaggio conversione da confrontare con il progetto di modello generato.

## <a name="configure-the-site-to-use-mvc"></a>Configurare il sito per l'uso di MVC

* Installare il `Microsoft.AspNetCore.Mvc` e `Microsoft.AspNetCore.StaticFiles` pacchetti NuGet.

  `Microsoft.AspNetCore.Mvc`è il framework ASP.NET MVC di base. `Microsoft.AspNetCore.StaticFiles`è il gestore di file statici. Il runtime di ASP.NET è modulare e deve esplicitamente ad utilizzati file statici (vedere [utilizzo di file statici](../fundamentals/static-files.md)).

* Aprire il *csproj* file (pulsante destro del mouse sul progetto in **Esplora** e selezionare **WebApp1.csproj modifica**) e aggiungere un `PrepareForPublish` destinazione:

  [!code-xml[Main](mvc/sample/WebApp1.csproj?range=21-23)]

  Il `PrepareForPublish` destinazione è necessaria per l'acquisizione delle librerie sul lato client tramite Bower. Parleremo che in un secondo momento.

* Aprire il *Startup.cs* file e modificare il codice in modo che corrisponda a quanto segue:

  [!code-csharp[Main](mvc/sample/Startup.cs?highlight=14,27-34)]

  Il `UseStaticFiles` metodo di estensione viene aggiunto il gestore di file statici. Come accennato in precedenza, il runtime di ASP.NET è modulare e deve esplicitamente ad utilizzati file statici. Il `UseMvc` consente di aggiungere il metodo di estensione di routing. Per ulteriori informazioni, vedere [avvio dell'applicazione](../fundamentals/startup.md) e [Routing](../fundamentals/routing.md).

## <a name="add-a-controller-and-view"></a>Aggiungere un controller e una visualizzazione

In questa sezione si aggiungerà un controller di minima e una visualizzazione per essere utilizzati come segnaposti per il controller MVC ASP.NET e le viste che è possibile eseguire la migrazione nella sezione successiva.

* Aggiungere un *controller* cartella.

* Aggiungere un **classe controller MVC** con il nome *HomeController.cs* per il *controller* cartella.

![Finestra di dialogo Aggiungi nuovo elemento](mvc/_static/add_mvc_ctl.png)

* Aggiungere un *viste* cartella.

* Aggiungere un *Views/Home* cartella.

* Aggiungere un *cshtml* pagina visualizzazione MVC per il *Views/Home* cartella.

![Finestra di dialogo Aggiungi nuovo elemento](mvc/_static/view.png)

La struttura del progetto è illustrata di seguito:

![Esplora soluzioni con file e cartelle di WebApp1](mvc/_static/project-structure-controller-view.png)

Sostituire il contenuto del *Views/Home/Index.cshtml* file con le operazioni seguenti:

```html
<h1>Hello world!</h1>
```

Eseguire l'app.

![Applicazione Web aperto in Microsoft Edge](mvc/_static/hello-world.png)

Vedere [controller](../mvc/controllers/index.md) e [viste](../mvc/views/index.md) per ulteriori informazioni.

Ora che è disponibile un progetto ASP.NET Core lavoro minimo, è possibile avviare la migrazione di funzionalità dal progetto ASP.NET MVC. È necessario spostare le operazioni seguenti:

* contenuto sul lato client (CSS, i tipi di carattere e script)

* controller

* visualizzazioni

* modelli

* Creazione di bundle

* filtri

* Log in/out, identità (questa operazione verrà eseguita nella prossima esercitazione.)

## <a name="controllers-and-views"></a>Controller e visualizzazioni

* Copia di ciascuno dei metodi di ASP.NET MVC `HomeController` al nuovo `HomeController`. Si noti che in ASP.NET MVC, tipo restituito del metodo del modello predefinito controller azione è [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET MVC di base, i metodi di azione restituiti `IActionResult` invece. `ActionResult`implementa `IActionResult`, pertanto non è necessario modificare il tipo restituito dei metodi di azione.

* Copia il *About.cshtml*, *Contact.cshtml*, e *cshtml* Visualizza file dal progetto ASP.NET MVC Razor per il progetto ASP.NET Core.

* Eseguire l'applicazione ASP.NET di base e ogni metodo di test. È non ancora eseguita la migrazione di file di layout o gli stili, in modo le viste visualizzabili conterrà solo il contenuto nel file di visualizzazione. Non sarà possibile i collegamenti generati file di layout per il `About` e `Contact` viste, pertanto è necessario essere richiamati dal browser (sostituire **4492** con il numero di porta usato nel progetto).

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Pagina contatto](mvc/_static/contact-page.png)

Si noti la mancanza di stile e voci di menu. Che verrà risolto nella sezione successiva.

## <a name="static-content"></a>Contenuto statico

Nelle versioni precedenti di ASP.NET MVC, contenuto statico è stato ospitato dalla radice del progetto web e stato combinato con i file sul lato server. In ASP.NET Core, contenuto statico è ospitato nel *wwwroot* cartella. Sarà necessario copiare il contenuto statico dall'app ASP.NET MVC precedente per il *wwwroot* cartella del progetto ASP.NET Core. In questa conversione di esempio:

* Copia il *favicon.ico* file dal progetto MVC precedente per il *wwwroot* cartella del progetto ASP.NET Core.

ASP.NET MVC il vecchio progetto usa [Bootstrap](http://getbootstrap.com/) lo stile e archivia il programma di avvio file nel *contenuto* e *script* cartelle. Il modello, che ha generato il vecchio progetto ASP.NET MVC, fa riferimento nel file di layout Bootstrap (*Views/Shared/_Layout.cshtml*). È possibile copiare il *bootstrap.js* e *bootstrap.css* di ASP.NET MVC i file di progetto per il *wwwroot* cartella in cui il nuovo progetto, ma tale approccio non utilizza il meccanismo migliore per la gestione delle dipendenze sul lato client in ASP.NET Core.

Nel nuovo progetto, verrà aggiunto il supporto per l'avvio e altre librerie sul lato client tramite [Bower](https://bower.io/):

* Aggiungi un [Bower](https://bower.io/) file di configurazione denominato *bower. JSON* alla radice del progetto (pulsante destro del mouse sul progetto, quindi **Aggiungi > Nuovo elemento > File di configurazione Bower**). Aggiungere [Bootstrap](http://getbootstrap.com/) e [jQuery](https://jquery.com/) al file (vedere le righe evidenziate riportato di seguito).

  [!code-json[Main](mvc/sample/bower.json?highlight=5-6)]

Dopo il salvataggio del file, Bower scaricherà automaticamente le dipendenze per il *wwwroot/lib* cartella. È possibile utilizzare il **Cerca in Esplora soluzioni** per trovare il percorso delle risorse:

![Asset jQuery visualizzati nei risultati della ricerca di Esplora soluzioni](mvc/_static/search.png)

Vedere [Gestisci pacchetti sul lato Client con Bower](../client-side/bower.md) per ulteriori informazioni.

<a name="migrate-layout-file"></a>

## <a name="migrate-the-layout-file"></a>La migrazione del file di layout

* Copia il *viewstart* file dal progetto ASP.NET MVC precedente *viste* nella cartella del progetto ASP.NET Core *viste* cartella. Il *viewstart* file non è stato modificato in ASP.NET MVC di base.

* Creare un *Views/Shared* cartella.

* *Facoltativo:* copia *viewimports. cshtml* dal *FullAspNetCore* del progetto MVC *viste* nella cartella del progetto ASP.NET Core *Viste* cartella. Rimuovere le dichiarazioni dello spazio dei nomi nella *viewimports. cshtml* file. Il *viewimports. cshtml* fornisce gli spazi dei nomi per tutti i file di visualizzazione e la porta file [gli helper di Tag](../mvc/views/tag-helpers/index.md). Gli helper di tag vengono utilizzati nel nuovo file di layout. Il *viewimports. cshtml* file è una novità di ASP.NET Core.

* Copia il *layout. cshtml* file dal progetto ASP.NET MVC precedente *Views/Shared* nella cartella del progetto ASP.NET Core *Views/Shared* cartella.

Aprire *layout. cshtml* file e apportare le modifiche seguenti (il codice completo è illustrato di seguito):

   * Sostituire `@Styles.Render("~/Content/css")` con un `<link>` elemento da cui caricare *bootstrap.css* (vedere sotto).

   * Rimuovere `@Scripts.Render("~/bundles/modernizr")`.

   * Impostare come commento il `@Html.Partial("_LoginPartial")` riga (racchiudono la riga con `@*...*@`). Verrà restituiamo ad esso in un'esercitazione future.

   * Sostituire `@Scripts.Render("~/bundles/jquery")` con un `<script>` elemento (vedere sotto).

   * Sostituire `@Scripts.Render("~/bundles/bootstrap")` con un `<script>` elemento (vedere sotto)...

Il collegamento CSS sostituzione:

```html
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
```

I tag di script di sostituzione:

```html
<script src="~/lib/jquery/dist/jquery.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
```

L'aggiornamento *layout. cshtml* file è illustrato di seguito:

[!code-html[Main](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7,27,39-40)]

Visualizzare il sito nel browser. Ora debba caricare correttamente, con gli stili previsto sul posto.

* *Facoltativo:* è possibile provare a usare il nuovo file di layout. Per questo progetto è possibile copiare il file di layout dal *FullAspNetCore* progetto. Il nuovo file di layout Usa [gli helper di Tag](../mvc/views/tag-helpers/index.md) e dispone di altri miglioramenti.

## <a name="configure-bundling--minification"></a>Configurare l'aggregazione & minimizzazione

Per informazioni su come configurare l'aggregazione e riduzione, vedere [Bundling and Minification](../client-side/bundling-and-minification.md).

## <a name="solving-http-500-errors"></a>Risolvere gli errori HTTP 500

Esistono molti problemi che possono causare un messaggio di errore HTTP 500 che non contengono informazioni sull'origine del problema. Ad esempio, se il *Views/_ViewImports.cshtml* file contiene uno spazio dei nomi che non esiste nel progetto, si otterrà un errore HTTP 500. Per ottenere un messaggio di errore dettagliato, aggiungere il codice seguente:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    if (env.IsDevelopment())
    {
         app.UseDeveloperExceptionPage();
    }

    app.UseStaticFiles();

    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

Vedere **utilizzando la pagina di eccezione Developer** in [Error Handling](../fundamentals/error-handling.md) per ulteriori informazioni.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Sviluppo lato client](../client-side/index.md)

* [Helper tag](../mvc/views/tag-helpers/index.md)
