---
title: Usare i servizi JavaScript per creare applicazioni a pagina singola in ASP.NET Core
author: scottaddie
description: Informazioni sui vantaggi derivanti dall'uso dei servizi JavaScript per creare un'applicazione a pagina singola (SPA) supportata da ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 09/06/2019
uid: client-side/spa-services
ms.openlocfilehash: c0c73882afd579510ad9cdf5b485c1d6fbeadd1c
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663779"
---
# <a name="use-javascript-services-to-create-single-page-applications-in-aspnet-core"></a>Usare i servizi JavaScript per creare applicazioni a pagina singola in ASP.NET Core

Di [Scott Addie](https://github.com/scottaddie) e [Fiyaz Hasan](https://fiyazhasan.me/)

Un applicazione a pagina singola (SPA) è un tipo comune di applicazione web a causa l'esperienza utente avanzata inerente. L'integrazione di Framework o librerie di SPA lato client, ad esempio [angolare](https://angular.io/) o [React](https://facebook.github.io/react/), con framework lato server come ASP.NET Core può essere difficile. Servizi JavaScript è stato sviluppato per ridurre l'attrito nel processo di integrazione. In questo modo l'operazione senza problemi tra i diversi client e gli stack di tecnologie di server.

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> Le funzionalità descritte in questo articolo sono obsolete a partire da ASP.NET Core 3,0. Un meccanismo di integrazione di Framework SPA più semplice è disponibile nel pacchetto NuGet [Microsoft. AspNetCore. SpaServices. Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices.Extensions) . Per ulteriori informazioni, vedere [[annuncio] Obsoleting Microsoft. AspNetCore. SpaServices e Microsoft. AspNetCore. NodeServices](https://github.com/dotnet/AspNetCore/issues/12890).

::: moniker-end

## <a name="what-is-javascript-services"></a>Informazioni sui servizi JavaScript

JavaScript Services è una raccolta di tecnologie lato client per ASP.NET Core. L'obiettivo consiste nel posizionare ASP.NET Core come piattaforma del lato server per compilazione SPAs preferita degli sviluppatori.

Servizi JavaScript è costituito da due pacchetti NuGet distinti:

* [Microsoft. AspNetCore. NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)
* [Microsoft. AspNetCore. SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)

Questi pacchetti sono utili negli scenari seguenti:

* Eseguire JavaScript nel server
* Usare un framework SPA o una raccolta
* Creare gli asset client-side con Webpack

Gran parte dello stato attivo in questo articolo viene posizionata sull'uso del pacchetto SpaServices.

## <a name="what-is-spaservices"></a>Che cos'è SpaServices

SpaServices è stato creato per posizionare ASP.NET Core come piattaforma del lato server per compilazione SPAs preferita degli sviluppatori. SpaServices non è necessario per sviluppare Spa con ASP.NET Core e non blocca gli sviluppatori in un particolare framework client.

SpaServices fornisce utili dell'infrastruttura, ad esempio:

* [Prerendering lato server](#server-side-prerendering)
* [Middleware dev per Webpack](#webpack-dev-middleware)
* [Sostituzione del modulo attivo](#hot-module-replacement)
* [Helper di routing](#routing-helpers)

Complessivamente, questi componenti di infrastruttura migliorano sia il flusso di lavoro di sviluppo e l'esperienza di runtime. I componenti possono essere adottati singolarmente.

## <a name="prerequisites-for-using-spaservices"></a>Prerequisiti per l'uso SpaServices

Per usare SpaServices, installare quanto segue:

* [Node. js](https://nodejs.org/) (versione 6 o successiva) con NPM

  * Per verificare questi componenti vengono installati e sono disponibili, eseguire il comando seguente dalla riga di comando:

    ```console
    node -v && npm -v
    ```

  * Se si esegue la distribuzione in un sito Web di Azure, non è necessaria alcuna azione&mdash;node. js è installato e disponibile negli ambienti server.

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * In Windows con Visual Studio 2017, l'SDK viene installato selezionando il carico di lavoro **sviluppo multipiattaforma .NET Core** .

* Pacchetto NuGet [Microsoft. AspNetCore. SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/)

## <a name="server-side-prerendering"></a>Rendering lato server preliminare

Un'applicazione (noto anche come siano isomorfi) universale è un'applicazione JavaScript in grado di eseguire sia nel server e client. Angular, React e altri framework comuni fornire una piattaforma universale per questo stile di sviluppo dell'applicazione. L'idea è prima di tutto eseguire il rendering i componenti del framework sul server tramite Node. js e delegare quindi ulteriormente l'esecuzione nel client.

ASP.NET Core [Helper Tag](xref:mvc/views/tag-helpers/intro) forniti da SpaServices semplificano l'implementazione del prerendering lato server richiamando le funzioni JavaScript sul server.

### <a name="server-side-prerendering-prerequisites"></a>Prerequisiti per il rendering lato server

Installare il pacchetto NPM per l'esecuzione del [prerendering di ASPNET](https://www.npmjs.com/package/aspnet-prerendering) :

```console
npm i -S aspnet-prerendering
```

### <a name="server-side-prerendering-configuration"></a>Configurazione del prerendering lato server

Gli helper tag vengono resi individuabili tramite la registrazione dello spazio dei nomi nel file *_ViewImports. cshtml* del progetto:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

Questi helper Tag estraggono le complessità di comunicare direttamente con le API di basso livello tramite una sintassi simile a HTML all'interno della visualizzazione Razor:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="asp-prerender-module-tag-helper"></a>ASP-PreRender-Helper tag del modulo

Il `asp-prerender-module` Helper tag, usato nell'esempio di codice precedente, esegue *ClientApp/dist/Main-server. js* sul server tramite node. js. Per maggiore chiarezza, il file *Main-server. js* è un artefatto dell'attività Transpilazione da typescript a JavaScript nel processo di compilazione [Webpack](https://webpack.github.io/) . Webpack definisce un alias del punto di ingresso di `main-server`; e l'attraversamento del grafico delle dipendenze per questo alias inizia dal file *ClientApp/boot-server. TS* :

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

Nell'esempio angolare seguente il file *ClientApp/boot-server. TS* usa la funzione `createServerRenderer` e il tipo di `RenderResult` del pacchetto di `aspnet-prerendering` NPM per configurare il rendering del server tramite node. js. Il markup HTML destinato al rendering lato server viene passato a una chiamata di funzione Resolve, che è racchiusa in un oggetto `Promise` JavaScript fortemente tipizzato. Il significato dell'oggetto `Promise` è che fornisce in modo asincrono il markup HTML alla pagina per l'inserimento nell'elemento segnaposto del DOM.

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="asp-prerender-data-tag-helper"></a>ASP-PreRender-Helper tag di dati

Se abbinato all'helper Tag `asp-prerender-module`, l'helper Tag `asp-prerender-data` può essere usato per passare le informazioni contestuali dalla visualizzazione Razor al codice JavaScript sul lato server. Il markup seguente, ad esempio, passa i dati utente al modulo `main-server`:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

L'argomento received `UserName` viene serializzato usando il serializzatore JSON incorporato e viene archiviato nell'oggetto `params.data`. Nell'esempio angolare seguente i dati vengono usati per creare un messaggio di saluto personalizzato all'interno di un elemento `h1`:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

I nomi delle proprietà passati negli Helper tag sono rappresentati con la notazione **PascalCase** . A differenza di JavaScript, in cui gli stessi nomi di proprietà sono rappresentati con **CamelCase**. La configurazione di serializzazione JSON di predefinita è responsabile di questa differenza.

Per espandersi sull'esempio di codice precedente, è possibile passare i dati dal server alla visualizzazione idratando la proprietà `globals` fornita alla funzione `resolve`:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

La matrice `postList` definita all'interno dell'oggetto `globals` è collegata all'oggetto `window` globale del browser. Questa variabile sottraendo in ambito globale consente di eliminare la duplicazione del lavoro richiesto, in particolare per quanto riguarda il caricamento dei dati stessi una sola volta nel server e nuovamente sul client.

![variabile globale postList collegato all'oggetto finestra](spa-services/_static/global_variable.png)

## <a name="webpack-dev-middleware"></a>Dev Middleware Webpack

Il [middleware dev per Webpack](https://webpack.js.org/guides/development/#using-webpack-dev-middleware) introduce un flusso di lavoro di sviluppo semplificato, in base al quale Webpack crea risorse su richiesta. Il middleware viene compilato automaticamente e gestisce le risorse lato client quando una pagina viene ricaricata nel browser. L'approccio alternativo consiste nel richiamare manualmente Webpack tramite script di compilazione del progetto npm quando viene modificata una dipendenza di terze parti o il codice personalizzato. Nell'esempio seguente viene illustrato uno script di compilazione NPM nel file *Package. JSON* :

```json
"build": "npm run build:vendor && npm run build:custom",
```

### <a name="webpack-dev-middleware-prerequisites"></a>Prerequisiti del middleware dev per Webpack

Installare il pacchetto NPM [ASPNET-Webpack](https://www.npmjs.com/package/aspnet-webpack) :

```console
npm i -D aspnet-webpack
```

### <a name="webpack-dev-middleware-configuration"></a>Configurazione di Webpack dev middleware

Il middleware dev per Webpack viene registrato nella pipeline di richieste HTTP tramite il codice seguente nel metodo `Configure` del file *Startup.cs* :

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_WebpackMiddlewareRegistration&highlight=4)]

Prima di [registrare l'hosting di file statici](xref:fundamentals/static-files) tramite il metodo di estensione `UseStaticFiles`, è necessario chiamare il metodo di estensione `UseWebpackDevMiddleware`. Per motivi di sicurezza, registrare il middleware solo quando l'app viene eseguita in modalità di sviluppo.

La proprietà `output.publicPath` del file *Webpack. config. js* indica al middleware di controllare la `dist` cartella per le modifiche:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

## <a name="hot-module-replacement"></a>Sostituzione di un modulo di accesso frequente

Si pensi alla funzionalità HMR ( [Hot Module Replacement](https://webpack.js.org/concepts/hot-module-replacement/) ) di Webpack come un'evoluzione del [middleware dev di Webpack](#webpack-dev-middleware). HMR introduce gli stessi vantaggi, ma semplifica ulteriormente il flusso di lavoro di sviluppo aggiornando automaticamente il contenuto della pagina dopo aver compilato le modifiche. Non confonderlo con l'aggiornamento del browser, che potrebbe interferire con la stato in memoria corrente e la sessione di debug di SPA. È presente un collegamento in tempo reale tra il servizio di Dev Middleware Webpack e il browser, ovvero vengono effettuato il push delle modifiche nel browser.

### <a name="hot-module-replacement-prerequisites"></a>Prerequisiti per la sostituzione dei moduli sensibili

Installare il pacchetto NPM per [Webpack-Hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) :

```console
npm i -D webpack-hot-middleware
```

### <a name="hot-module-replacement-configuration"></a>Configurazione della sostituzione del modulo attivo

Il componente HMR deve essere registrato nella pipeline di richieste HTTP di MVC nel metodo `Configure`:

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

Come è stato fatto con il [middleware dev per Webpack](#webpack-dev-middleware), è necessario chiamare il metodo di estensione `UseWebpackDevMiddleware` prima del `UseStaticFiles` metodo di estensione. Per motivi di sicurezza, registrare il middleware solo quando l'app viene eseguita in modalità di sviluppo.

Il file *Webpack. config. js* deve definire una matrice di `plugins`, anche se viene lasciata vuota:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

Dopo aver caricato l'app nel browser, scheda Console degli strumenti di sviluppo offre conferma dell'attivazione HMR:

![Messaggio di connessione di sostituzione di un modulo a caldo](spa-services/_static/hmr_connected.png)

## <a name="routing-helpers"></a>Helper di routing

Nella maggior parte delle applicazioni Spa basate su ASP.NET Core, il routing lato client spesso è necessario oltre al routing lato server. I sistemi di routing SPA e MVC possono lavorare in modo indipendente senza interferenze. È presente, tuttavia, sono le problematiche ponendo un di case edge: identificazione delle risposte HTTP 404.

Si consideri lo scenario in cui viene utilizzata una route con estensione `/some/page`. Si supponga che la richiesta non modello-match una route sul lato server, ma il criterio di ricerca corrisponde a una route lato client. Si consideri ora una richiesta in ingresso per `/images/user-512.png`, che in genere prevede di trovare un file di immagine nel server. Se il percorso della risorsa richiesto non corrisponde ad alcuna route sul lato server o a un file statico, è improbabile che l'applicazione sul lato client la gestisca&mdash;in genere la restituzione di un codice di stato HTTP 404.

### <a name="routing-helpers-prerequisites"></a>Prerequisiti per il routing degli helper

Installare il pacchetto NPM di routing lato client. Utilizzo di Angular ad esempio:

```console
npm i -S @angular/router
```

### <a name="routing-helpers-configuration"></a>Configurazione degli helper di routing

Un metodo di estensione denominato `MapSpaFallbackRoute` viene usato nel metodo `Configure`:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_MvcRoutingTable&highlight=7-9)]

Le route vengono valutate nell'ordine in cui sono state configurate. Di conseguenza, il `default` route nell'esempio di codice precedente viene usato per primo per i criteri di ricerca.

## <a name="create-a-new-project"></a>Creare un nuovo progetto

I servizi JavaScript forniscono modelli di applicazione preconfigurati. SpaServices viene usato in questi modelli insieme a Framework e librerie diversi, ad esempio angolare, React e Redux.

Questi modelli possono essere installati tramite la CLI di .NET Core, eseguire il comando seguente:

```dotnetcli
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

Viene visualizzato un elenco dei modelli di applicazione a singola pagina disponibili:

| Modelli                                 | Nome breve | Linguaggio | Tag        |
| ------------------------------------------| :--------: | :------: | :---------: |
| MVC ASP.NET Core con Angular             | angular    | [C#]     | Web/MVC/SPA |
| MVC ASP.NET Core con React. js            | react      | [C#]     | Web/MVC/SPA |
| MVC ASP.NET Core con React.js e Redux  | reactredux | [C#]     | Web/MVC/SPA |

Per creare un nuovo progetto usando uno dei modelli di SPA, includere il **nome breve** del modello nel comando [DotNet New](/dotnet/core/tools/dotnet-new) . Il comando seguente crea un'applicazione Angular con ASP.NET Core MVC configurata per il lato server:

```dotnetcli
dotnet new angular
```

### <a name="set-the-runtime-configuration-mode"></a>Impostare la modalità di configurazione di runtime

Sono disponibili due modalità di configurazione di runtime principale:

* **Sviluppo**:
  * Include i mapping di origine a fini di debug.
  * Non ottimizzare il codice lato client per le prestazioni.
* **Produzione**:
  * Esclude il mapping di origine.
  * Ottimizza il codice lato client tramite la creazione di bundle e minification.

ASP.NET Core usa una variabile di ambiente denominata `ASPNETCORE_ENVIRONMENT` per archiviare la modalità di configurazione. Per ulteriori informazioni, vedere [impostazione dell'ambiente](xref:fundamentals/environments#set-the-environment).

### <a name="run-with-net-core-cli"></a>Esegui con interfaccia della riga di comando di .NET Core

Ripristinare i pacchetti npm e NuGet necessari eseguendo il comando seguente nella radice del progetto:

```dotnetcli
dotnet restore && npm i
```

Compilare ed eseguire l'applicazione:

```dotnetcli
dotnet run
```

L'applicazione viene avviata in localhost in base alla [modalità di configurazione del runtime](#set-the-runtime-configuration-mode). Passando a `http://localhost:5000` nel browser viene visualizzata la pagina di destinazione.

### <a name="run-with-visual-studio-2017"></a>Eseguire con Visual Studio 2017

Aprire il file con *estensione csproj* generato dal comando [DotNet New](/dotnet/core/tools/dotnet-new) . I pacchetti NuGet e npm necessari vengono ripristinati automaticamente al progetto aperto. Questo processo di ripristino può richiedere alcuni minuti e l'applicazione è pronta per l'esecuzione quando viene completato. Fare clic sul pulsante verde Run (Esegui) oppure premere `Ctrl + F5`. il browser si apre alla pagina di destinazione dell'applicazione. L'applicazione viene eseguita in localhost in base alla [modalità di configurazione del runtime](#set-the-runtime-configuration-mode).

## <a name="test-the-app"></a>Testare l'app

I modelli SpaServices sono preconfigurati per eseguire test sul lato client usando [Karma](https://karma-runner.github.io/1.0/index.html) e [Jasmine](https://jasmine.github.io/). Jasmine è un diffuso framework di unit test per JavaScript, mentre Karma è un test runner per tali test. Karma è configurato per funzionare con il [middleware dev di Webpack](#webpack-dev-middleware) in modo che lo sviluppatore non debba arrestare ed eseguire il test ogni volta che vengono apportate modifiche. Se si tratta del codice in esecuzione per il test case o test case stesso, il test viene eseguito automaticamente.

Usando l'applicazione angolare come esempio, sono già disponibili due test case Jasmine per la `CounterComponent` nel file *Counter. Component. spec. TS* :

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

Aprire il prompt dei comandi nella directory *ClientApp* Eseguire il comando seguente:

```console
npm test
```

Lo script avvia Karma Test Runner, che legge le impostazioni definite nel file *Karma. conf. js* . Tra le altre impostazioni, *Karma. conf. js* identifica i file di test da eseguire tramite la relativa matrice di `files`:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

## <a name="publish-the-app"></a>Pubblicare l'app

Per altre informazioni sulla pubblicazione in Azure, vedere [questo problema di GitHub](https://github.com/dotnet/AspNetCore.Docs/issues/12474) .

La combinazione degli asset generati dal lato client e gli elementi pubblicati in ASP.NET Core in un pacchetto pronto per la distribuzione può essere complessa. Fortunatamente, SpaServices Orchestra l'intero processo di pubblicazione con una destinazione MSBuild personalizzata denominata `RunWebpack`:

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

La destinazione MSBuild ha le responsabilità seguenti:

1. Ripristinare i pacchetti NPM.
1. Creare una build di livello produzione degli asset lato client di terze parti.
1. Creare una compilazione di livello di produzione degli asset personalizzati sul lato client.
1. Copiare gli asset generati da Webpack nella cartella di pubblicazione.

La destinazione MSBuild viene richiamata durante l'esecuzione:

```dotnetcli
dotnet publish -c Release
```

## <a name="additional-resources"></a>Risorse aggiuntive

* [Documenti angolari](https://angular.io/docs)
