---
title: Bundle e minifiy asset statici in ASP.NET Core
author: scottaddie
description: Informazioni su come ottimizzare le risorse statiche in un'applicazione web ASP.NET Core applicando le tecniche di aggregazione e la riduzione.
ms.author: scaddie
ms.custom: mvc
ms.date: 01/10/2018
uid: client-side/bundling-and-minification
ms.openlocfilehash: ae9836a6ad0ff0bc834bf2eb10ff5fd97c3c659a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279571"
---
# <a name="bundle-and-minifiy-static-assets-in-aspnet-core"></a>Bundle e minifiy asset statici in ASP.NET Core

Di [Scott Addie](https://twitter.com/Scott_Addie)

Questo articolo illustra i vantaggi dell'applicazione come aggregare e minimizzazione, incluso come queste funzionalità possono essere utilizzate con le applicazioni web ASP.NET Core.

## <a name="what-is-bundling-and-minification"></a>Che cos'è l'aggregazione e minimizzazione?

Creazione di bundle e minimizzazione sono due le ottimizzazioni delle prestazioni di distinct che è possibile applicare in un'app web. Utilizzati insieme, aggregazione e minimizzazione migliorare le prestazioni riducendo il numero di richieste di server e ridurre le dimensioni delle risorse statiche richieste.

Come aggregare e minimizzazione principalmente migliorare il tempo di caricamento di prima pagina richiesta. Una volta che una pagina web è stato richiesto, il browser memorizza nella cache le risorse statiche (JavaScript, CSS e immagini). Di conseguenza, aggregazione e riduzione non migliorare le prestazioni quando si richiede la stessa pagina, o le pagine, nello stesso sito richiede le stesse risorse. Se la scadenza di intestazione non è impostata correttamente le risorse in e se non viene usata l'aggregazione e riduzione, approccio euristico di aggiornamento del browser contrassegna le risorse non aggiornati dopo alcuni giorni. Inoltre, il browser richiede una richiesta di convalida per ogni asset. In questo caso, aggregazione e minimizzazione forniscono un miglioramento delle prestazioni anche dopo la prima richiesta di pagina.

### <a name="bundling"></a>Creazione di bundle

Creazione di bundle combina più file in un singolo file. Creazione di bundle riduce il numero di richieste di server necessari eseguire il rendering di una risorsa web, ad esempio una pagina web. È possibile creare un numero qualsiasi di singoli pacchetti in modo specifico per CSS, JavaScript e così via. Un numero inferiore di file significa meno le richieste HTTP dal browser al server o dal servizio fornendo all'applicazione. I risultati miglioramento delle prestazioni di caricamento prima pagina.

### <a name="minification"></a>Minimizzazione

Minimizzazione rimuove i caratteri non necessari dal codice senza modificare la funzionalità. Il risultato è una riduzione di grandi dimensioni in asset richiesti (ad esempio CSS, immagini e i file JavaScript). Gli effetti collaterali di minimizzazione includono abbreviare i nomi delle variabili per un carattere e la rimozione di commenti e gli spazi vuoti non necessari.

Si consideri la funzione JavaScript seguente:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

Minimizzazione riduce la funzione per le operazioni seguenti:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

Oltre a rimuovere i commenti e gli spazi vuoti non necessari, i nomi di parametri e variabili seguenti sono stati rinominati come indicato di seguito:

Originale | Ridenominazione
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a>Impatto di aggregazione e di riduzione

Nella tabella seguente vengono descritte le differenze tra il caricamento delle risorse singolarmente e utilizzando l'aggregazione e la riduzione:

Operazione | Con B/M | Senza B/M | Modifica
--- | :---: | :---: | :---:
Richieste di file  | 7   | 18     | 157%
KB trasferiti | 156 | 264.68 | 70%
Tempo di caricamento (ms) | 885 | 2360   | 167%

Browser sono piuttosto dettagliati per quanto riguarda le intestazioni di richiesta HTTP. I byte totali inviati metrica visto una significativa riduzione durante l'aggregazione. Il tempo di caricamento Mostra un miglioramento significativo, ma in questo esempio viene eseguito localmente. Miglioramenti delle prestazioni superiori vengono realizzati quando l'utilizzo di aggregazione e minimizzazione asset trasferiti su una rete.

## <a name="choose-a-bundling-and-minification-strategy"></a>Scegliere una strategia di riduzione e di aggregazione

I modelli di progetto MVC e pagine Razor forniscono una soluzione di casella per la creazione di bundle e minimizzazione costituito da un file di configurazione JSON. Strumenti di terze parti, ad esempio il [Gulp](xref:client-side/using-gulp) e [Grunt](xref:client-side/using-grunt) attività canali, eseguire le stesse attività con una maggiore complessità. Uno strumento di terze parti è un'ottima soluzione quando il flusso di lavoro di sviluppo richiede l'elaborazione oltre l'aggregazione e minimizzazione&mdash;, ad esempio ottimizzazione linting e immagine. Usando come aggregare in fase di progettazione e la riduzione, vengono creati i file minimizzati prima della distribuzione dell'app. Aggiunta e la riduzione prima della distribuzione offre il vantaggio del carico del server ridotto. Tuttavia, è importante tenere presente che in fase di progettazione di aggregazione e minimizzazione aumenta la complessità di compilazione e funziona solo con file statici.

## <a name="configure-bundling-and-minification"></a>Configurare l'aggregazione e riduzione

I modelli di progetto MVC e pagine Razor forniscono un *bundleconfig.json* file di configurazione che definisce le opzioni per ogni pacchetto. Per impostazione predefinita, viene modificata una configurazione di pacchetto per il codice JavaScript personalizzato (*wwwroot/js/site.js*) e fogli di stile (*wwwroot/css/site.css*) file:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

Opzioni di configurazione includono:

* `outputFileName`: Il nome del file di bundle all'output. Può contenere un percorso relativo di *bundleconfig.json* file. **Obbligatorio**
* `inputFiles`: Una matrice di file da raggruppare. Questi sono i percorsi relativi al file di configurazione. **parametro facoltativo**, * comporta un valore vuoto in un file di output vuoto. [il glob](http://www.tldp.org/LDP/abs/html/globbingref.html) sono supportati i modelli.
* `minify`: Le opzioni di riduzione per il tipo di output. **parametro facoltativo**, *impostazione predefinita: `minify: { enabled: true }`*
  * Opzioni di configurazione sono disponibili per ogni tipo di file di output.
    * [Minimizzatore CSS](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [Minimizzatore JavaScript](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [Minimizzatore HTML](https://github.com/madskristensen/BundlerMinifier/wiki)
* `includeInProject`: Flag che indica se aggiungere i file generati file di progetto. **parametro facoltativo**, *predefinito: false*
* `sourceMap`: Flag che indica se generare una mappa di origine per il file aggregato. **parametro facoltativo**, *predefinito: false*
* `sourceMapRootPath`: Il percorso radice per l'archiviazione dei file di mappa di origine generato.

## <a name="build-time-execution-of-bundling-and-minification"></a>Esecuzione della fase di compilazione di aggregazione e di riduzione

Il [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) pacchetto NuGet consente l'esecuzione dell'aggregazione e riduzione in fase di compilazione. Inserisce il pacchetto [destinazioni di MSBuild](/visualstudio/msbuild/msbuild-targets) che eseguita in compilazione e l'ora pulita. Il *bundleconfig.json* file viene analizzato dal processo di compilazione per generare i file di output in base alla configurazione definita.

> [!NOTE]
> BuildBundlerMinifier appartiene a un progetto basato sulla community su GitHub per il quale Microsoft non fornisce supporto. Devono essere presentati problemi [qui](https://github.com/madskristensen/BundlerMinifier/issues).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Aggiungere il *BuildBundlerMinifier* pacchetto al progetto.

Compilare il progetto. Di seguito viene visualizzata nella finestra di Output:

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>  Minified wwwroot/css/site.min.css
1>  Minified wwwroot/js/site.min.js
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

Pulire il progetto. Di seguito viene visualizzata nella finestra di Output:

```console
1>------ Clean started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>
1>Bundler: Cleaning output from bundleconfig.json
1>Bundler: Done cleaning output file from bundleconfig.json
========== Clean: 1 succeeded, 0 failed, 0 skipped ==========
```

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli) 

Aggiungere il *BuildBundlerMinifier* pacchetto al progetto:

```console
dotnet add package BuildBundlerMinifier
```

Se si utilizza ASP.NET Core 1. x, ripristinare il pacchetto appena aggiunto:

```console
dotnet restore
```

Compilare il progetto:

```console
dotnet build
```

Viene visualizzato quanto segue:

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


    Bundler: Begin processing bundleconfig.json
    Bundler: Done processing bundleconfig.json
    BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
```

Pulire il progetto:

```console
dotnet clean
```

Viene visualizzato il seguente output:

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a>Esecuzione ad hoc di aggregazione e di riduzione

È possibile eseguire le attività di creazione di bundle e riduzione in base ad hoc, senza compilare il progetto. Aggiungere il [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) pacchetto NuGet al progetto:

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> BundlerMinifier.Core appartiene a un progetto basato sulla community su GitHub per il quale Microsoft non fornisce supporto. Devono essere presentati problemi [qui](https://github.com/madskristensen/BundlerMinifier/issues).

Questo pacchetto estende l'interfaccia CLI Core .NET per includere il *dotnet bundle* strumento. Nella finestra della Console di gestione di pacchetti (PMC) o in una shell dei comandi, è possibile eseguire il comando seguente:

```console
dotnet bundle
```

> [!IMPORTANT]
> Gestione pacchetti NuGet aggiunge le dipendenze per il file *. csproj come `<PackageReference />` nodi. Il `dotnet bundle` comando è registrato con l'interfaccia CLI Core .NET solo quando un `<DotNetCliToolReference />` viene utilizzato il nodo. Modificare di conseguenza il file *. csproj.

## <a name="add-files-to-workflow"></a>Aggiungere file al flusso di lavoro

Si consideri un esempio in cui un ulteriore *custom.css* aggiunta di file simile a quello riportato di seguito:

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

Per minimizzare *custom.css* e aggregare con *site.css* in un *site.min.css* file, aggiungere il relativo percorso *bundleconfig.json*:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> In alternativa, può essere utilizzato il modello il glob seguente:
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]
> ```
>
> Questo modello il glob corrisponde a tutti i file CSS ed esclude il modello di file minimizzata.

Compilare l'applicazione. Aprire *site.min.css* e osservare il contenuto di *custom.css* viene aggiunto alla fine del file.

## <a name="environment-based-bundling-and-minification"></a>Basato sull'ambiente come aggregare e riduzione

Come procedura consigliata, i file di bundle e minimizzati dell'app devono essere utilizzati in un ambiente di produzione. Durante lo sviluppo, i file originali apportare per semplificare il debug dell'app.

Specificare i file da includere nelle pagine usando il [Helper di Tag di ambiente](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) nelle visualizzazioni. L'Helper di Tag di ambiente solo viene eseguito il rendering del relativo contenuto durante l'esecuzione di specifiche [ambienti](xref:fundamentals/environments).

Nell'esempio `environment` tag esegue il rendering di file CSS non elaborati durante l'esecuzione nel `Development` ambiente:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

---

Nell'esempio `environment` tag esegue il rendering di file CSS in dotazione e minimizzati durante l'esecuzione in un ambiente diverso da `Development`. Ad esempio, in esecuzione in `Production` o `Staging` attiva il rendering di tali fogli di stile:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

---

## <a name="consume-bundleconfigjson-from-gulp"></a>Utilizzare bundleconfig.json da Gulp

Vi sono casi in cui come aggregare e minimizzazione flusso di lavoro un'app richiede un'ulteriore elaborazione. Sono esempi di ottimizzazione dell'immagine, busting cache e l'elaborazione di risorse della rete CDN. Per soddisfare questi requisiti, è possibile convertire il flusso di lavoro come aggregare e riduzione per l'utilizzo di Gulp.

### <a name="use-the-bundler--minifier-extension"></a>Utilizzare l'estensione di Bundler & Minimizzatore

Visual Studio [Bundler & Minimizzatore](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) estensione gestisce la conversione a Gulp.

> [!NOTE]
> L'estensione di Bundler & Minimizzatore appartiene a un progetto basato sulla community su GitHub per il quale Microsoft non fornisce supporto. Devono essere presentati problemi [qui](https://github.com/madskristensen/BundlerMinifier/issues).

Fare doppio clic su di *bundleconfig.json* file in Esplora soluzioni e selezionare **Bundler & Minimizzatore** > **convertire a Gulp...** :

![Menu di scelta rapida per convertire Gulp](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

Il *gulpfile.js* e *package. JSON* file vengono aggiunti al progetto. Supporto [npm](https://www.npmjs.com/) pacchetti elencati nella *package. JSON* file `devDependencies` sezione vengono installati.

Nella finestra PMC per installare l'interfaccia CLI Gulp come una dipendenza globale, eseguire il comando seguente:

```console
npm i -g gulp-cli
```

Il *gulpfile.js* file legge il *bundleconfig.json* file per l'input, output e le impostazioni.

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a>Convertire manualmente

Se Visual Studio e/o l'estensione di Bundler & Minimizzatore non è disponibile, convertire manualmente.

Aggiungere un *package. JSON* file, con i seguenti `devDependencies`, alla radice del progetto:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

Installare le dipendenze eseguendo il comando seguente allo stesso livello *package. JSON*:

```console
npm i
```

Installare l'interfaccia CLI Gulp come dipendenza globale:

```console
npm i -g gulp-cli
```

Copia il *gulpfile.js* file sotto la radice del progetto:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a>Esecuzione di attività Gulp

Per attivare l'attività di riduzione Gulp prima del progetto in Visual Studio, aggiungere il seguente [destinazione MSBuild](/visualstudio/msbuild/msbuild-targets) al file *. csproj:

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

In questo esempio, tutte le attività definite all'interno di `MyPreCompileTarget` destinazione eseguire prima predefiniti `Build` destinazione. Output simile al seguente viene visualizzato nella finestra di Output di Visual Studio:

```console
1>------ Build started: Project: BuildBundlerMinifierApp, Configuration: Debug Any CPU ------
1>BuildBundlerMinifierApp -> C:\BuildBundlerMinifierApp\bin\Debug\netcoreapp2.0\BuildBundlerMinifierApp.dll
1>[14:17:49] Using gulpfile C:\BuildBundlerMinifierApp\gulpfile.js
1>[14:17:49] Starting 'min:js'...
1>[14:17:49] Starting 'min:css'...
1>[14:17:49] Starting 'min:html'...
1>[14:17:49] Finished 'min:js' after 83 ms
1>[14:17:49] Finished 'min:css' after 88 ms
========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
```

In alternativa, Esplora esecuzione attività di Visual Studio può essere utilizzato per associare attività Gulp a eventi specifici di Visual Studio. Vedere [esecuzione di attività predefinito](xref:client-side/using-gulp#running-default-tasks) per istruzioni sull'esecuzione di operazioni con.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Usare Gulp](xref:client-side/using-gulp)
* [Usare Grunt](xref:client-side/using-grunt)
* [Usare più ambienti](xref:fundamentals/environments)
* [Helper tag](xref:mvc/views/tag-helpers/intro)
