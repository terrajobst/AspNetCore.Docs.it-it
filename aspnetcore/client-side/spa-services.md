---
title: Consente di creare applicazioni a pagina singola in ASP.NET Core JavaScriptServices
author: scottaddie
description: Informazioni sui vantaggi dell'utilizzo JavaScriptServices per creare un'applicazione a pagina singola (SPA) supportato da ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/spa-services
ms.openlocfilehash: 05b0d7f31e167e620f2d168109ffd907ba120a49
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
# <a name="use-javascriptservices-to-create-single-page-applications-in-aspnet-core"></a>Consente di creare applicazioni a pagina singola in ASP.NET Core JavaScriptServices

Da [Scott Addie](https://github.com/scottaddie) e [Fiyaz Hasan](http://fiyazhasan.me/)

Un'applicazione a pagina singola (SPA) è un tipo comune di applicazione web a causa l'esperienza utente avanzata inerente. Integrazione Framework SPA o raccolte, sul lato client, ad esempio [angolare](https://angular.io/) o [reagire](https://facebook.github.io/react/), con Framework sul lato server quali ASP.NET Core può essere difficile. [JavaScriptServices](https://github.com/aspnet/JavaScriptServices) è stato sviluppato per ridurre le forze di attrito nel processo di integrazione. In questo modo l'operatività tra diversi client e gli stack di tecnologie di server.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a>Che cos'è JavaScriptServices?

JavaScriptServices è una raccolta di tecnologie lato client per ASP.NET Core. L'obiettivo consiste nel posizionare ASP.NET Core come piattaforma lato server preferito degli sviluppatori per la compilazione di SPAs.

JavaScriptServices è costituita da tre pacchetti NuGet distinti:
* [Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)
* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)
* [Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)

Questi pacchetti sono utili se si:
* Eseguire JavaScript nel server
* Utilizzare un framework SPA o una raccolta
* Compilazione di risorse sul lato client con Webpack

Gran parte dello stato attivo in questo articolo viene posizionata sull'uso del pacchetto SpaServices.

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a>Che cos'è SpaServices?

SpaServices è stato creato per posizionare ASP.NET Core come piattaforma lato server preferito degli sviluppatori per la compilazione di SPAs. SpaServices non è necessario sviluppare SPAs con ASP.NET Core e non è del blocco in un framework client specifico.

SpaServices fornisce infrastruttura utile, ad esempio:
* [Rendering preliminare sul lato server](#server-prerendering)
* [Dev Middleware Webpack](#webpack-dev-middleware)
* [Sostituzione di un modulo a caldo](#hot-module-replacement)
* [Helper di routing](#routing-helpers)

Complessivamente, questi componenti dell'infrastruttura migliorano sia il flusso di lavoro di sviluppo e il runtime. I componenti possono essere adottati singolarmente.

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a>Prerequisiti per l'utilizzo SpaServices

Per utilizzare SpaServices, installare quanto segue:
* [Node.js](https://nodejs.org/) (versione 6 o versione successiva) con npm
  * Per verificare questi componenti sono installati ed sono disponibile, eseguire il comando seguente dalla riga di comando:

    ```console
    node -v && npm -v
    ```

Nota: Se si distribuisce in un sito web di Azure, non è necessario effettuare alcuna operazione qui &mdash; Node.js è installato e disponibile negli ambienti server.

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * Se si è in Windows tramite Visual Studio 2017, il SDK viene installato selezionando il **lo sviluppo multipiattaforma con .NET Core** carico di lavoro.

* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) pacchetto NuGet

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a>Rendering preliminare sul lato server

Un'applicazione universale (noto anche come siano isomorfi) è un'applicazione JavaScript in grado di eseguire sia sul server e client. Angolare React e altri framework comuni offre una piattaforma universale per questo stile di sviluppo di applicazioni. L'idea è prima di eseguire il rendering i componenti del framework sul server tramite Node.js e quindi delegare ulteriormente l'esecuzione del client.

ASP.NET Core [gli helper di Tag](xref:mvc/views/tag-helpers/intro) fornito da SpaServices semplifica l'implementazione del rendering lato server preliminare chiamando le funzioni JavaScript nel server.

### <a name="prerequisites"></a>Prerequisiti

Installare gli elementi seguenti:
* [il rendering preliminare di ASPNET](https://www.npmjs.com/package/aspnet-prerendering) pacchetto npm:

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a>Configurazione

Gli helper di Tag resi individuabili tramite la registrazione dello spazio dei nomi del progetto *viewimports. cshtml* file:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

Questi helper di Tag di astrarre le complessità di comunicare direttamente con l'API di basso livello tramite una sintassi simile a HTML all'interno della visualizzazione Razor:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a>Il `asp-prerender-module` Helper Tag

Il `asp-prerender-module` Helper di Tag, utilizzato nell'esempio di codice precedente, esegue *ClientApp/dist/main-server.js* sul server tramite Node.js. Per chiarezza, *main server.js* file è un elemento dell'attività transpilation TypeScript e JavaScript il [Webpack](http://webpack.github.io/) processo di compilazione. Webpack definisce un alias di punto di ingresso di `main-server`; e l'attraversamento di grafico delle dipendenze per questo alias inizia in corrispondenza di *ClientApp/avvio-server.ts* file:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

Nell'esempio seguente angolare il *ClientApp/avvio-server.ts* file utilizza il `createServerRenderer` (funzione) e `RenderResult` tipo del `aspnet-prerendering` pacchetto npm da configurare per il rendering di server tramite Node.js. Il markup HTML destinato per il rendering lato server viene passato a una chiamata di funzione di risoluzione, viene eseguito il wrapping in un JavaScript fortemente tipizzato `Promise` oggetto. Il `Promise` è di importanza dell'oggetto in modo asincrono che fornisce il markup HTML per la pagina per l'aggiunta nell'elemento segnaposto del DOM.

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a>Il `asp-prerender-data` Helper Tag

Quando è associato il `asp-prerender-module` Helper di Tag, il `asp-prerender-data` Helper di Tag può essere usato per passare informazioni contestuali dalla visualizzazione Razor per il codice JavaScript sul lato server. Ad esempio, il markup seguente passa i dati utente per il `main-server` modulo:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

I dati ricevuti `UserName` argomento viene serializzato utilizzando il serializzatore JSON incorporato e viene archiviato nel `params.data` oggetto. Nell'esempio seguente angolare, i dati vengono utilizzati per costruire un messaggio di saluto personalizzato all'interno di un `h1` elemento:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

Nota: I nomi delle proprietà passati gli helper di Tag sono rappresentati con **PascalCase** notazione. Invece che a JavaScript, in cui gli stessi nomi di proprietà sono rappresentati con **camelCase**. La configurazione predefinita della serializzazione JSON è responsabile di questa differenza.

Per espandere l'esempio di codice precedente, dati possono essere passati dal server per la visualizzazione da idratare le il `globals` proprietà fornita per il `resolve` funzione:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

Il `postList` matrice definita all'interno di `globals` oggetto viene connesso al browser globale `window` oggetto. Questa variabile sottraendo in ambito globale elimina la duplicazione di impegno, in particolare per quanto attiene per il caricamento dei dati stessi, una volta sul server e di nuovo nel client.

![variabile postList globale associata all'oggetto finestra](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a>Dev Middleware Webpack

[Dev Middleware Webpack](https://webpack.github.io/docs/webpack-dev-middleware.html) introduce un flusso di lavoro di sviluppo ottimizzato in base al quale Webpack compila le risorse su richiesta. Il middleware automaticamente compila e serve le risorse sul lato client quando una pagina viene ricaricata nel browser. L'approccio alternativo consiste nel richiamare manualmente Webpack tramite script di compilazione del progetto npm quando viene modificato il codice personalizzato o una dipendenza di terze parti. Script di compilazione un npm *package. JSON* file è illustrato nell'esempio seguente:

[!code-json[](../client-side/spa-services/sample/SpaServicesSampleApp/package.json?range=5)]

### <a name="prerequisites"></a>Prerequisiti

Installare gli elementi seguenti:
* [ASPNET webpack](https://www.npmjs.com/package/aspnet-webpack) pacchetto npm:

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a>Configurazione

Dev Middleware Webpack è registrato nella pipeline delle richieste HTTP tramite il codice seguente nel *Startup.cs* file `Configure` metodo:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

Il `UseWebpackDevMiddleware` metodo di estensione deve essere chiamato prima [la registrazione di file statici hosting](xref:fundamentals/static-files) tramite il `UseStaticFiles` metodo di estensione. Per motivi di sicurezza, è possibile registrare il middleware solo quando l'app in esecuzione in modalità di sviluppo.

Il *webpack.config.js* file `output.publicPath` proprietà indica il middleware per controllare il `dist` cartella per le modifiche:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a>Sostituzione di un modulo a caldo

Si consideri di Webpack [sostituzione di un modulo a caldo](https://webpack.github.io/docs/hot-module-replacement-with-webpack.html) funzionalità (HMR) come un'evoluzione del [Webpack Dev Middleware](#webpack-dev-middleware). HMR introduce gli stessi vantaggi, ma ulteriormente semplifica il flusso di lavoro di sviluppo aggiornando automaticamente il contenuto della pagina dopo la compilazione delle modifiche. Non confondere con un aggiornamento del browser, che potrebbe interferire con la stato in memoria corrente e la sessione di debug di SPA. È presente un collegamento in tempo reale tra il servizio Webpack Dev Middleware e il browser, il che significa che le modifiche vengono inviate al browser.

### <a name="prerequisites"></a>Prerequisiti

Installare gli elementi seguenti:
* [middleware hot webpack](https://www.npmjs.com/package/webpack-hot-middleware) pacchetto npm:

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a>Configurazione

Il componente HMR deve essere registrato in pipeline di richieste HTTP di MVC nel `Configure` metodo:

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

Come è stato true con [Webpack Dev Middleware](#webpack-dev-middleware), `UseWebpackDevMiddleware` metodo di estensione deve essere chiamato prima il `UseStaticFiles` metodo di estensione. Per motivi di sicurezza, è possibile registrare il middleware solo quando l'app in esecuzione in modalità di sviluppo.

Il *webpack.config.js* file è necessario definire un `plugins` matrice, anche se viene lasciata vuota:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

Dopo il caricamento dell'app nel browser, scheda Console gli strumenti di sviluppo fornisce conferma dell'attivazione HMR:

![Messaggio di connessione a caldo di sostituzione di un modulo](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a>Helper di routing

Nella maggior parte dei SPAs basate su ASP.NET Core, è opportuno lato client routing oltre al routing lato server. I sistemi di routing SPA e MVC possono lavorare in modo indipendente senza interferenze. Se è presente, tuttavia, un bordo case che presentano problemi trovano ad affrontare: identificazione delle risposte HTTP 404.

Si consideri lo scenario in cui una route senza estensione di `/some/page` viene utilizzato. Si supponga che la richiesta non-corrispondenza una route sul lato server, ma il criterio di corrispondenza con una route sul lato client. Ora si consideri una richiesta in ingresso per `/images/user-512.png`, che in genere prevede di trovare un file di immagine nel server. Se il percorso della risorsa richiesta non corrisponde a qualsiasi route sul lato server o un file statico, è improbabile che l'applicazione sul lato client si gestisce, in genere si desidera restituire un codice di stato HTTP 404.

### <a name="prerequisites"></a>Prerequisiti

Installare gli elementi seguenti:
* Il pacchetto npm routing sul lato client. Ad esempio, utilizzando angolare:

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a>Configurazione

Un metodo di estensione denominato `MapSpaFallbackRoute` viene utilizzata la `Configure` metodo:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

Suggerimento: Le route vengono valutate nell'ordine in cui la configurazione di tali. Di conseguenza, il `default` route nell'esempio di codice precedente prima viene utilizzata per criteri di ricerca.

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a>Crea un nuovo progetto

JavaScriptServices fornisce modelli di applicazione configurati in precedenza. SpaServices viene usato in questi modelli, unitamente a diversi Framework e librerie, ad esempio angolare Aurelia, Knockout, React e dotato.

Questi modelli possono essere installati tramite l'interfaccia CLI Core .NET eseguendo il comando seguente:

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

Viene visualizzato un elenco dei modelli SPA disponibili:

| Modelli                                 | Nome breve | Linguaggio | Tag        |
|:------------------------------------------|:-----------|:---------|:------------|
| MVC ASP.NET Core con Angular             | angular    | [C#]     | Web/MVC/SPA |
| MVC ASP.NET Core con Aurelia             | aurelia    | [C#]     | Web/MVC/SPA |
| MVC ASP.NET Core con Knockout.js         | knockout   | [C#]     | Web/MVC/SPA |
| MVC ASP.NET Core con React.js            | react      | [C#]     | Web/MVC/SPA |
| MVC ASP.NET Core con React.js e il ritorno  | reactredux | [C#]     | Web/MVC/SPA |
| MVC ASP.NET Core con Vue.js              | dotato        | [C#]     | Web/MVC/SPA | 

Per creare un nuovo progetto utilizzando uno dei modelli di SPA, inclusa la **nome breve** del modello nella [dotnet nuovo](/dotnet/core/tools/dotnet-new) comando. Il comando seguente crea un'applicazione angolare con ASP.NET MVC di base configurato per il lato server:

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a>Impostare la modalità di configurazione di runtime

Esistono due modalità di configurazione di runtime principale:
* **Sviluppo**:
    * Include i mapping di origine per facilitare il debug.
    * Non ottimizzare il codice sul lato client per le prestazioni.
* **Produzione**:
    * Esclude i mapping di origine.
    * Ottimizza il codice sul lato client tramite l'aggregazione e di riduzione.

ASP.NET Core utilizza una variabile di ambiente denominata `ASPNETCORE_ENVIRONMENT` per archiviare la modalità di configurazione. Vedere **[l'impostazione dell'ambiente](xref:fundamentals/environments#setting-the-environment)** per ulteriori informazioni.

### <a name="running-with-net-core-cli"></a>In esecuzione con .NET Core CLI

Ripristinare il NuGet necessarie e i pacchetti npm eseguendo il comando seguente nella radice del progetto:

```console
dotnet restore && npm i
```

Compilare ed eseguire l'applicazione:

```console
dotnet run
```

Avvio dell'applicazione su localhost in base al [modalità di configurazione di runtime](#runtime-config-mode). Passare a `http://localhost:5000` nel browser verrà visualizzata la pagina di destinazione.

### <a name="running-with-visual-studio-2017"></a>In esecuzione con Visual Studio 2017

Aprire il *csproj* file generato dal [dotnet nuovo](/dotnet/core/tools/dotnet-new) comando. I pacchetti NuGet e npm richiesti vengono ripristinati automaticamente al progetto aperto. Questo processo di ripristino potrebbe richiedere alcuni minuti e l'applicazione è pronta per l'esecuzione quando viene completato. Fare clic sul pulsante Esegui verde o premere `Ctrl + F5`, e il browser si apre alla pagina di destinazione dell'applicazione. L'applicazione viene eseguita in localhost in base al [modalità di configurazione di runtime](#runtime-config-mode). 

<a name="app-testing"></a>

## <a name="testing-the-app"></a>Test dell'app

I modelli SpaServices sono configurati in precedenza per eseguire test sul lato client tramite [Karma](https://karma-runner.github.io/1.0/index.html) e [Jasmine](https://jasmine.github.io/). Jasmine è un diffuso framework unit test per JavaScript, mentre Karma è un test runner per i test. Karma è configurato per funzionare con il [Webpack Dev Middleware](#webpack-dev-middleware) tale che lo sviluppatore non è necessario arrestare ed eseguire il test ogni volta che vengono apportate modifiche. Se si tratta del codice in esecuzione per il test case o test case stesso, il test viene eseguito automaticamente.

Utilizzando l'applicazione angolare ad esempio, due casi di prova al gelsomino sono già forniti per il `CounterComponent` nel *counter.component.spec.ts* file:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

Aprire il prompt dei comandi nella radice del progetto ed eseguire il comando seguente:

```console
npm test
```

Lo script avvia il Karma test runner, che legge le impostazioni definite nel *karma.conf.js* file. Tra le altre impostazioni, il *karma.conf.js* identifica i file di test da eseguire tramite il relativo `files` matrice:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a>Pubblicazione dell'applicazione

Combinando le risorse sul lato client generate e gli elementi di ASP.NET Core pubblicati in un pacchetto pronte per l'utilizzo può essere complesso. Fortunatamente, SpaServices Orchestra tale processo intera pubblicazione con una destinazione di MSBuild personalizzata denominata `RunWebpack`:

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

La destinazione MSBuild ha le responsabilità seguenti:
1. Ripristinare i pacchetti npm
1. Crea una build di livello di produzione gli asset di terze parti, sul lato client
1. Creare una build di produzione di livello di risorse sul lato client personalizzate
1. Copiare le risorse Webpack generato nella cartella di pubblicazione

La destinazione MSBuild viene richiamata durante l'esecuzione:

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a>Risorse aggiuntive

* [Documenti Angular](https://angular.io/docs)
