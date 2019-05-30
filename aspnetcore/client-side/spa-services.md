---
title: Utilizzare i servizi JavaScript per creare applicazioni a pagina singola in ASP.NET Core
author: scottaddie
description: Scopri i vantaggi dell'uso di servizi di JavaScript per creare un applicazione a pagina singola (SPA) supportati da ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 05/28/2019
uid: client-side/spa-services
ms.openlocfilehash: 2ac7a6c7a5499d0525a61c6401f5996f8543fdba
ms.sourcegitcommit: 4d05e30567279072f1b070618afe58ae1bcefd5a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376216"
---
# <a name="use-javascript-services-to-create-single-page-applications-in-aspnet-core"></a>Utilizzare i servizi JavaScript per creare applicazioni a pagina singola in ASP.NET Core

Dal [Scott Addie](https://github.com/scottaddie) e [Fiyaz Hasan](http://fiyazhasan.me/)

Un applicazione a pagina singola (SPA) è un tipo comune di applicazione web a causa l'esperienza utente avanzata inerente. L'integrazione Framework SPA o librerie lato client, ad esempio [Angular](https://angular.io/) oppure [React](https://facebook.github.io/react/), con framework lato server, ad esempio ASP.NET Core può essere difficile. I servizi JavaScript è stato sviluppato per ridurre gli attriti nel processo di integrazione. In questo modo l'operazione senza problemi tra i diversi client e gli stack di tecnologie di server.

## <a name="what-is-javascript-services"></a>Che cos'è servizi JavaScript

I servizi JavaScript è un insieme di tecnologie lato client per ASP.NET Core. L'obiettivo consiste nel posizionare ASP.NET Core come piattaforma del lato server per compilazione SPAs preferita degli sviluppatori.

Servizi di JavaScript è costituito da tre pacchetti NuGet distinti:

* [Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)
* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)

Questi pacchetti sono utili negli scenari seguenti:

* Eseguire JavaScript nel server
* Usare un framework SPA o una raccolta
* Creare gli asset client-side con Webpack

Gran parte dello stato attivo in questo articolo viene posizionata sull'uso del pacchetto SpaServices.

## <a name="what-is-spaservices"></a>Che cos'è SpaServices

SpaServices è stato creato per posizionare ASP.NET Core come piattaforma del lato server per compilazione SPAs preferita degli sviluppatori. Per sviluppare SPAs con ASP.NET Core non è necessario SpaServices e gli sviluppatori non venga bloccato in un framework client specifico.

SpaServices fornisce utili dell'infrastruttura, ad esempio:

* [Rendering lato server preliminare](#server-side-prerendering)
* [Dev Middleware Webpack](#webpack-dev-middleware)
* [Sostituzione di un modulo di accesso frequente](#hot-module-replacement)
* [Helper di routing](#routing-helpers)

Complessivamente, questi componenti di infrastruttura migliorano sia il flusso di lavoro di sviluppo e l'esperienza di runtime. I componenti possono essere adottati singolarmente.

## <a name="prerequisites-for-using-spaservices"></a>Prerequisiti per l'uso SpaServices

Per usare SpaServices, installare quanto segue:

* [Node. js](https://nodejs.org/) (versione 6 o versione successiva) con npm

  * Per verificare questi componenti vengono installati e sono disponibili, eseguire il comando seguente dalla riga di comando:

    ```console
    node -v && npm -v
    ```

  * Se la distribuzione in un sito web di Azure, è necessaria alcuna azione&mdash;Node. js sia installato e disponibile negli ambienti server.

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * In Windows usando Visual Studio 2017, il SDK è installato, selezionando il **sviluppo multipiattaforma .NET Core** carico di lavoro.

* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) pacchetto NuGet

## <a name="server-side-prerendering"></a>Rendering lato server preliminare

Un'applicazione (noto anche come siano isomorfi) universale è un'applicazione JavaScript in grado di eseguire sia nel server e client. Angular, React e altri framework comuni fornire una piattaforma universale per questo stile di sviluppo dell'applicazione. L'idea è prima di tutto eseguire il rendering i componenti del framework sul server tramite Node. js e delegare quindi ulteriormente l'esecuzione nel client.

ASP.NET Core [helper Tag](xref:mvc/views/tag-helpers/intro) fornita da SpaServices semplificano l'implementazione del rendering lato server preliminare chiamando le funzioni di JavaScript nel server.

### <a name="server-side-prerendering-prerequisites"></a>Prerequisiti di pre-rendering lato server

Installare il [aspnet-il rendering preliminare](https://www.npmjs.com/package/aspnet-prerendering) pacchetto npm:

```console
npm i -S aspnet-prerendering
```

### <a name="server-side-prerendering-configuration"></a>Configurazione pre-rendering lato server

Gli helper Tag resi individuabili tramite la registrazione dello spazio dei nomi del progetto *viewimports. cshtml* file:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

Questi helper Tag estraggono le complessità di comunicare direttamente con le API di basso livello tramite una sintassi simile a HTML all'interno della visualizzazione Razor:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="asp-prerender-module-tag-helper"></a>Helper di Tag ASP-prerender-module

Il `asp-prerender-module` Helper Tag, usati nell'esempio di codice precedente, viene eseguito *ClientApp/dist/main-server.js* sul server tramite Node. js. Per chiarezza, *main-server. js* file è un elemento dell'attività volta TypeScript e JavaScript il [Webpack](http://webpack.github.io/) processo di compilazione. Webpack definisce un alias di punto di ingresso del `main-server`; e, attraversamento del grafico delle dipendenze per questo alias inizia in corrispondenza di *ClientApp/avvio-server.ts* file:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

Nell'esempio seguente Angular, il *ClientApp/avvio-server.ts* file utilizza il `createServerRenderer` (funzione) e `RenderResult` digitare del `aspnet-prerendering` pacchetto npm per configurare il rendering di server tramite Node. js. Il markup HTML destinato per il rendering lato server viene passato a una chiamata di funzione di risoluzione, che viene eseguito il wrapping in un JavaScript fortemente tipizzato `Promise` oggetto. Il `Promise` significato dell'oggetto è in modo asincrono che fornisce il markup HTML per la pagina per l'inserimento nell'elemento segnaposto del DOM.

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="asp-prerender-data-tag-helper"></a>Helper di Tag ASP-prerender-data

Se usato in combinazione con il `asp-prerender-module` Helper Tag, il `asp-prerender-data` Helper Tag può essere utilizzato per passare informazioni contestuali dalla vista Razor per JavaScript lato server. Ad esempio, il markup seguente passa i dati utente per il `main-server` modulo:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

I dati ricevuti `UserName` argomenti viene serializzato utilizzando il serializzatore JSON predefinito e viene archiviato nel `params.data` oggetto. Nell'esempio seguente Angular, i dati vengono utilizzati per costruire un messaggio di saluto personalizzato all'interno di un `h1` elemento:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

I nomi delle proprietà passati gli helper Tag sono rappresentati con **PascalCase** notation. Ciò contrasta in JavaScript, in cui gli stessi nomi di proprietà sono rappresentati con **camelCase**. La configurazione di serializzazione JSON di predefinita è responsabile di questa differenza.

Per espandere l'esempio di codice precedente, i dati possono essere passati dal server per la visualizzazione da idratare le il `globals` proprietà fornite per il `resolve` funzione:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

Il `postList` matrice definita all'interno di `globals` oggetto è connesso al browser globale `window` oggetto. Questa variabile sottraendo in ambito globale consente di eliminare la duplicazione del lavoro richiesto, in particolare per quanto riguarda il caricamento dei dati stessi una sola volta nel server e nuovamente sul client.

![variabile globale postList collegato all'oggetto finestra](spa-services/_static/global_variable.png)

## <a name="webpack-dev-middleware"></a>Dev Middleware Webpack

[Dev Middleware Webpack](https://webpack.js.org/guides/development/#using-webpack-dev-middleware) introduce un flusso di lavoro di sviluppo semplificato in base al quale Webpack compila risorse su richiesta. Il middleware viene compilato automaticamente e gestisce le risorse lato client quando una pagina viene ricaricata nel browser. L'approccio alternativo consiste nel richiamare manualmente Webpack tramite script di compilazione del progetto npm quando viene modificata una dipendenza di terze parti o il codice personalizzato. Script di compilazione un npm *package. JSON* file è illustrato nell'esempio seguente:

```json
"build": "npm run build:vendor && npm run build:custom",
```

### <a name="webpack-dev-middleware-prerequisites"></a>Prerequisiti di Webpack Dev Middleware

Installare il [aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) pacchetto npm:

```console
npm i -D aspnet-webpack
```

### <a name="webpack-dev-middleware-configuration"></a>Configurazione Webpack Dev Middleware

Webpack Dev Middleware è registrato nella pipeline delle richieste HTTP tramite il codice seguente nel *Startup.cs* del file `Configure` metodo:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_WebpackMiddlewareRegistration&highlight=4)]

Il `UseWebpackDevMiddleware` metodo di estensione deve essere chiamato prima [la registrazione dei file statici che ospita](xref:fundamentals/static-files) tramite il `UseStaticFiles` metodo di estensione. Per motivi di sicurezza, registrare il middleware solo quando l'app viene eseguita in modalità di sviluppo.

Il *webpack.config.js* del file `output.publicPath` proprietà indica al middleware di guardare il `dist` cartella per le modifiche:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

## <a name="hot-module-replacement"></a>Sostituzione di un modulo di accesso frequente

Può essere considerata di Webpack [modulo di sostituzione a caldo](https://webpack.js.org/concepts/hot-module-replacement/) funzionalità (HMR) come un'evoluzione del [Webpack Dev Middleware](#webpack-dev-middleware). HMR introduce gli stessi vantaggi, ma semplifica ulteriormente il flusso di lavoro di sviluppo aggiornando automaticamente il contenuto della pagina dopo aver compilato le modifiche. Non confonderlo con l'aggiornamento del browser, che potrebbe interferire con la stato in memoria corrente e la sessione di debug di SPA. È presente un collegamento in tempo reale tra il servizio di Dev Middleware Webpack e il browser, ovvero vengono effettuato il push delle modifiche nel browser.

### <a name="hot-module-replacement-prerequisites"></a>Prerequisiti di modulo sostituzione a caldo

Installare il [middleware hot webpack](https://www.npmjs.com/package/webpack-hot-middleware) pacchetto npm:

```console
npm i -D webpack-hot-middleware
```

### <a name="hot-module-replacement-configuration"></a>Configurazione di sostituzione dei moduli a caldo

Il componente HMR deve essere registrato nella pipeline di richieste HTTP di MVC nel `Configure` metodo:

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

Come era true con [Webpack Dev Middleware](#webpack-dev-middleware), il `UseWebpackDevMiddleware` metodo di estensione deve essere chiamato prima di `UseStaticFiles` metodo di estensione. Per motivi di sicurezza, registrare il middleware solo quando l'app viene eseguita in modalità di sviluppo.

Il *webpack.config.js* file necessario definire un `plugins` matrice, anche se viene lasciata vuota:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

Dopo aver caricato l'app nel browser, scheda Console degli strumenti di sviluppo offre conferma dell'attivazione HMR:

![Messaggio di connessione di sostituzione di un modulo a caldo](spa-services/_static/hmr_connected.png)

## <a name="routing-helpers"></a>Helper di routing

Nella maggior parte dei SPAs basate su ASP.NET Core, il routing lato client è spesso desiderato oltre al routing lato server. I sistemi di routing SPA e MVC possono lavorare in modo indipendente senza interferenze. È presente, tuttavia, sono le problematiche ponendo un di case edge: identificazione delle risposte HTTP 404.

Si consideri lo scenario in cui una route senza estensione di `/some/page` viene usato. Si supponga che la richiesta non modello-match una route sul lato server, ma il criterio di ricerca corrisponde a una route lato client. Ora si consideri una richiesta in ingresso per `/images/user-512.png`, che in genere prevede di trovare un file di immagine nel server. Se tale percorso risorsa richiesta non corrisponde a qualsiasi route sul lato server o dei file statici, è improbabile che l'applicazione lato client sarebbe gestirlo&mdash;è richiesta la restituzione in genere un codice di stato HTTP 404.

### <a name="routing-helpers-prerequisites"></a>Prerequisiti di helper routing

Installare il pacchetto npm routing lato client. Utilizzo di Angular ad esempio:

```console
npm i -S @angular/router
```

### <a name="routing-helpers-configuration"></a>Configurazione del routing helper

Un metodo di estensione denominato `MapSpaFallbackRoute` viene utilizzata la `Configure` metodo:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_MvcRoutingTable&highlight=7-9)]

Le route vengono valutate nell'ordine in cui vengono configurate. Di conseguenza, il `default` route nell'esempio di codice precedente prima viene utilizzata per criteri di ricerca.

## <a name="create-a-new-project"></a>Creare un nuovo progetto

Servizi di JavaScript forniscono modelli di applicazione configurati in precedenza. SpaServices viene usato in questi modelli in combinazione con diversi Framework e librerie, ad esempio Angular, React e Redux.

Questi modelli possono essere installati tramite la CLI di .NET Core, eseguire il comando seguente:

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

Viene visualizzato un elenco dei modelli di applicazione a singola pagina disponibili:

| Modelli                                 | Nome breve | Linguaggio | Tag        |
| ------------------------------------------| :--------: | :------: | :---------: |
| MVC ASP.NET Core con Angular             | angular    | [C#]     | Web/MVC/SPA |
| MVC ASP.NET Core con React. js            | react      | [C#]     | Web/MVC/SPA |
| MVC ASP.NET Core con React.js e Redux  | reactredux | [C#]     | Web/MVC/SPA |

Per creare un nuovo progetto usando uno dei modelli di applicazione a singola pagina, inclusa la **nome breve** del modello nel [dotnet nuovo](/dotnet/core/tools/dotnet-new) comando. Il comando seguente crea un'applicazione Angular con ASP.NET Core MVC configurata per il lato server:

```console
dotnet new angular
```

### <a name="set-the-runtime-configuration-mode"></a>Impostare la modalità di configurazione di runtime

Sono disponibili due modalità di configurazione di runtime principale:

* **Sviluppo**:
  * Include i mapping di origine a fini di debug.
  * Non ottimizzare il codice lato client per le prestazioni.
* **Ambiente di produzione**:
  * Esclude il mapping di origine.
  * Ottimizza il codice lato client tramite creazione di bundle e minimizzazione.

ASP.NET Core Usa variabile di ambiente denominata `ASPNETCORE_ENVIRONMENT` per archiviare la modalità di configurazione. Per altre informazioni, vedere [impostare l'ambiente](xref:fundamentals/environments#set-the-environment).

### <a name="run-with-net-core-cli"></a>Eseguire con .NET Core CLI

Ripristinare i pacchetti npm e NuGet necessari eseguendo il comando seguente nella radice del progetto:

```console
dotnet restore && npm i
```

Compilare ed eseguire l'applicazione:

```console
dotnet run
```

Avvio dell'applicazione sull'host locale in base al [modalità di configurazione di runtime](#set-the-runtime-configuration-mode). Passare a `http://localhost:5000` nel browser verrà visualizzata la pagina di destinazione.

### <a name="run-with-visual-studio-2017"></a>Eseguire con Visual Studio 2017

Aprire il *file con estensione csproj* file generato dal [dotnet nuovo](/dotnet/core/tools/dotnet-new) comando. I pacchetti NuGet e npm necessari vengono ripristinati automaticamente al progetto aperto. Questo processo di ripristino può richiedere alcuni minuti e l'applicazione è pronta per l'esecuzione quando viene completato. Fare clic sul pulsante di esecuzione verde o premere `Ctrl + F5`, e il browser si apre alla pagina di destinazione dell'applicazione. L'applicazione viene eseguita sull'host locale in base al [modalità di configurazione di runtime](#set-the-runtime-configuration-mode).

## <a name="test-the-app"></a>Eseguire il test dell'app

I modelli SpaServices sono preconfigurati per eseguire i test sul lato client, usando [Karma](https://karma-runner.github.io/1.0/index.html) e [Jasmine](https://jasmine.github.io/). Jasmine è un diffuso framework di unit test per JavaScript, mentre Karma è un test runner per tali test. Karma è configurato per funzionare con il [Webpack Dev Middleware](#webpack-dev-middleware) tale che lo sviluppatore non è necessario arrestare ed eseguire i test ogni volta che vengono apportate modifiche. Se si tratta del codice in esecuzione per il test case o test case stesso, il test viene eseguito automaticamente.

Usa l'applicazione Angular ad esempio, due Jasmine test case sono già fornite per il `CounterComponent` nella *counter.component.spec.ts* file:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

Aprire il prompt dei comandi nel *ClientApp* directory. Eseguire il comando seguente:

```console
npm test
```

Lo script avvia il test runner Karma, nel quale vengono lette le impostazioni definite nel *karma.conf.js* file. Tra le altre impostazioni, il *karma.conf.js* identifica i file di test da eseguire tramite relativo `files` matrice:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

## <a name="publish-the-app"></a>Pubblicare l'app

La combinazione degli asset generati dal lato client e gli elementi pubblicati in ASP.NET Core in un pacchetto pronto per la distribuzione può essere complessa. Per fortuna, SpaServices Orchestra processo intera pubblicazione con una destinazione di MSBuild personalizzata denominata `RunWebpack`:

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

La destinazione MSBuild ha le responsabilità seguenti:

1. Ripristinare i pacchetti npm.
1. Creare una build di livello di produzione gli asset lato client, di terze parti.
1. Creare una build di livello di produzione di risorse lato client personalizzate.
1. Copiare gli asset Webpack generato nella cartella di pubblicazione.

La destinazione MSBuild viene richiamata durante l'esecuzione:

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a>Risorse aggiuntive

* [Documentazione di Angular](https://angular.io/docs)
