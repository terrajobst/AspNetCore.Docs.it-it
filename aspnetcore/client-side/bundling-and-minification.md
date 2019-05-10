---
title: Aggregare e minimizzare asset statici in ASP.NET Core
author: scottaddie
description: Informazioni su come ottimizzare le risorse statiche in un'applicazione web ASP.NET Core applicando tecniche di creazione di bundle e minimizzazione.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/20/2018
uid: client-side/bundling-and-minification
ms.openlocfilehash: 5d5f0aadb7740c9b2b959d12a585cd8c91758ce8
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64894298"
---
# <a name="bundle-and-minify-static-assets-in-aspnet-core"></a>Aggregare e minimizzare asset statici in ASP.NET Core

Dal [Scott Addie](https://twitter.com/Scott_Addie) e [pino David](https://twitter.com/davidpine7)

Questo articolo illustra i vantaggi dell'applicazione e minimizzazione, tra cui come queste funzionalità possono essere utilizzate con le app web ASP.NET Core.

## <a name="what-is-bundling-and-minification"></a>Che cos'è l'aggregazione e minimizzazione

Creazione di bundle e minimizzazione sono due le ottimizzazioni delle prestazioni distinct che è possibile applicare in un'app web. Usati insieme, creazione di bundle e minimizzazione migliorare le prestazioni riducendo il numero di richieste al server e ridurre le dimensioni dei asset statici richiesti.

Creazione di bundle e minimizzazione principalmente migliorare il tempo di caricamento di prima pagina richiesta. Una volta che una pagina web è stato richiesto, il browser memorizza nella cache gli asset statici (JavaScript, CSS e immagini). Di conseguenza, creazione di bundle e minimizzazione non migliorare le prestazioni quando si richiede la stessa pagina, o pagine, nello stesso sito richiede le stesse risorse. Se la scadenza dell'intestazione non è impostato correttamente per le risorse e se non viene usata l'aggregazione e minimizzazione, l'euristica di aggiornamento del browser, contrassegnare gli asset non aggiornato dopo alcuni giorni. Inoltre, il browser richiede una richiesta di convalida per ogni asset. Creazione di bundle e minimizzazione in questo caso, fornire un miglioramento delle prestazioni anche dopo la prima richiesta di pagina.

### <a name="bundling"></a>Creazione di bundle

Creazione di bundle di combinare più file in un unico file. Creazione di bundle consente di ridurre il numero di richieste di server necessari eseguire il rendering di un asset web, ad esempio una pagina web. È possibile creare un numero qualsiasi di singoli pacchetti in modo specifico per CSS, JavaScript e così via. Minor numero di file significa meno le richieste HTTP dal browser al server o dal servizio che fornisce all'applicazione. Il risultato è migliorate le prestazioni di carico prima pagina.

### <a name="minification"></a>Minimizzazione

Minimizzazione rimuove caratteri non necessari dal codice senza modificare la funzionalità. Il risultato è una riduzione delle dimensioni significative asset richiesti (ad esempio CSS, immagini e file JavaScript). Gli effetti collaterali di minimizzazione includono ad abbreviare i nomi delle variabili per un carattere e la rimozione di commenti e spazi vuoti non necessari.

Si consideri la seguente funzione di JavaScript:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.js)]

Minimizzazione riduce la funzione al seguente:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/js/site.min.js)]

Oltre a rimuovere i commenti e spazi vuoti non necessari, i nomi di parametro e variabile seguenti sono stati rinominati come indicato di seguito:

Originale | Ridenominazione
--- | :---:
`imageTagAndImageID` | `t`
`imageContext` | `a`
`imageElement` | `r`

## <a name="impact-of-bundling-and-minification"></a>Impatto della creazione di bundle e minimizzazione

La tabella seguente descrive le differenze tra il caricamento di asset e utilizzo di creazione di bundle e minimizzazione singolarmente:

Operazione | Con B/M | Senza B/M | Modifica
--- | :---: | :---: | :---:
Richieste di file  | 7   | 18     | 157%
KB trasferiti | 156 | 264.68 | 70%
Tempo di caricamento (ms) | 885 | 2360   | 167%

I browser sono piuttosto dettagliati per quanto riguarda le intestazioni di richiesta HTTP. I byte totali inviati metrica compariva una riduzione significativa della creazione di bundle. Il tempo di caricamento Mostra un miglioramento significativo, ma in questo esempio viene eseguita in locale. Miglioramenti delle prestazioni maggiori si concretizzano con gli asset di creazione di bundle e minimizzazione trasferiti su una rete.

## <a name="choose-a-bundling-and-minification-strategy"></a>Scegliere una strategia di creazione di bundle e minimizzazione

I modelli di progetto MVC e Razor Pages forniscono una soluzione di out-of-the-box per la creazione di bundle e minimizzazione costituito da un file di configurazione JSON. Strumenti di terze parti, ad esempio la [Gulp](xref:client-side/using-gulp) e [Grunt](xref:client-side/using-grunt) gli strumenti di esecuzione di attività, eseguire le stesse attività con un po' più complesso. Uno strumento di terze parti può essere un candidato ideale quando il flusso di lavoro di sviluppo richiede ulteriori elaborazioni, oltre alla creazione di bundle e minimizzazione&mdash;, come l'ottimizzazione di Lint e l'immagine. Usando in fase di progettazione e minimizzazione, vengono creati i file minimizzati prima della distribuzione dell'app. Creazione di bundle e minimizzazione prima della distribuzione offre il vantaggio di carico del server ridotto. Tuttavia, è importante tenere presente che in fase di progettazione creazione di bundle e minimizzazione aumenta la complessità di compilazione e funziona solo con i file statici.

## <a name="configure-bundling-and-minification"></a>Configurare la creazione di bundle e minimizzazione

::: moniker range="<= aspnetcore-2.0"

In ASP.NET Core 2.0 o versioni precedenti, i modelli di progetto MVC e Razor Pages forniscono un *bundleconfig.json* file di configurazione che definisce le opzioni per ogni aggregazione:

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

In ASP.NET Core 2.1 o versione successiva, aggiungere un nuovo file JSON, denominato *bundleconfig.json*, alla radice del progetto MVC o le pagine Razor. Includere il codice JSON seguente nel file come punto di partenza:

::: moniker-end

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig.json)]

Il *bundleconfig.json* file definisce le opzioni per ogni aggregazione. Nell'esempio precedente, una configurazione singola aggregazione è definita per il codice JavaScript personalizzato (*wwwroot/js/site.js*) e fogli di stile (*wwwroot/css/site.css*) file.

Opzioni di configurazione includono:

* `outputFileName`: Il nome del file di bundle per l'output. Può contenere un percorso relativo di *bundleconfig.json* file. **required**
* `inputFiles`: Matrice di file da raccogliere. Questi sono i percorsi relativi nel file di configurazione. **facoltativo**, * restituisce un valore vuoto in un file di output vuoto. [glob](http://www.tldp.org/LDP/abs/html/globbingref.html) sono supportati i modelli.
* `minify`: Le opzioni di minimizzazione per il tipo di output. **optional**, *default - `minify: { enabled: true }`*
  * Opzioni di configurazione sono disponibili per ogni tipo di file di output.
    * [CSS Minifier](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [JavaScript Minifier](https://github.com/madskristensen/BundlerMinifier/wiki/JavaScript-Minifier-settings)
    * [HTML Minifier](https://github.com/madskristensen/BundlerMinifier/wiki)
* `includeInProject`: Flag che indica se aggiungere i file generati da file di progetto. **optional**, *default - false*
* `sourceMap`: Flag che indica se generare una mappa di origine per il file nel bundle. **optional**, *default - false*
* `sourceMapRootPath`: Il percorso radice per l'archiviazione file di mappa di origine generato.

## <a name="build-time-execution-of-bundling-and-minification"></a>Esecuzione fase di compilazione di creazione di bundle e minimizzazione

Il [BuildBundlerMinifier](https://www.nuget.org/packages/BuildBundlerMinifier/) pacchetto NuGet consente l'esecuzione di creazione di bundle e minimizzazione in fase di compilazione. Il pacchetto inserisce [destinazioni di MSBuild](/visualstudio/msbuild/msbuild-targets) che Esegui in compilazione e fase di pulizia. Il *bundleconfig.json* file viene analizzato dal processo di compilazione per generare i file di output in base alla configurazione definita.

> [!NOTE]
> BuildBundlerMinifier appartiene a un progetto in GitHub per il quale Microsoft non fornisce supporto basato sulla community. Problemi devono essere segnalati [qui](https://github.com/madskristensen/BundlerMinifier/issues).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Aggiungere il *BuildBundlerMinifier* pacchetto al progetto.

Compilare il progetto. Di seguito viene visualizzato nella finestra di Output:

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

Pulire il progetto. Di seguito viene visualizzato nella finestra di Output:

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

Se si usa ASP.NET Core 1.x, ripristinare il pacchetto appena aggiunto:

```console
dotnet restore
```

Compilare il progetto:

```console
dotnet build
```

Verrà visualizzato quanto segue:

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

Viene visualizzato l'output seguente:

```console
Microsoft (R) Build Engine version 15.4.8.50001 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Cleaning output from bundleconfig.json
  Bundler: Done cleaning output file from bundleconfig.json
```

---

## <a name="ad-hoc-execution-of-bundling-and-minification"></a>L'esecuzione ad hoc di creazione di bundle e minimizzazione

È possibile eseguire le attività di creazione di bundle e minimizzazione in base ad hoc, senza compilare il progetto. Aggiungere il [BundlerMinifier.Core](https://www.nuget.org/packages/BundlerMinifier.Core/) pacchetto NuGet al progetto:

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=10)]

> [!NOTE]
> BundlerMinifier.Core appartiene a un progetto in GitHub per il quale Microsoft non fornisce supporto basato sulla community. Problemi devono essere segnalati [qui](https://github.com/madskristensen/BundlerMinifier/issues).

Questo pacchetto estende la CLI di .NET Core per includere il *dotnet bundle* dello strumento. Nella finestra della Console di gestione pacchetti (PMC) o in una shell dei comandi, è possibile eseguire il comando seguente:

```console
dotnet bundle
```

> [!IMPORTANT]
> Gestione pacchetti NuGet aggiunge le dipendenze al file *.csproj come `<PackageReference />` nodi. Il `dotnet bundle` comando è registrato con la CLI di .NET Core solo quando un `<DotNetCliToolReference />` viene utilizzato il nodo. Modificare di conseguenza il file *. csproj.

## <a name="add-files-to-workflow"></a>Aggiungere file al flusso di lavoro

Si consideri un esempio in cui un'ulteriore *Custom. CSS* aggiunta di file simile a quello riportato di seguito:

[!code-css[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/wwwroot/css/custom.css)]

Per minimizzare *Custom. CSS* e creare un bundle con *Site. CSS* in un *site.min.css* Aggiungi il percorso relativo *bundleconfig.json*:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/bundleconfig2.json?highlight=6)]

> [!NOTE]
> In alternativa, può essere utilizzato il seguente criterio glob:
>
> ```json
> "inputFiles": ["wwwroot/**/*(*.css|!(*.min.css))"]
> ```
>
> Questo criterio glob corrisponde a tutti i file CSS ed esclude il modello di file minimizzati.

Compilare l'applicazione. Aprire *site.min.css* e notare che il contenuto del *Custom. CSS* viene aggiunto alla fine del file.

## <a name="environment-based-bundling-and-minification"></a>Basato sull'ambiente di creazione di bundle e minimizzazione

Come procedura consigliata, i file in bundle e minimizzati dell'app devono essere utilizzati in un ambiente di produzione. Durante lo sviluppo, i file originali apportare per semplificare il debug dell'app.

Specificare i file da includere nelle pagine usando il [Helper Tag di ambiente](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper) nelle viste. L'Helper Tag di ambiente il rendering solo il contenuto durante l'esecuzione in specifici [ambienti](xref:fundamentals/environments).

Quanto segue `environment` tag esegue il rendering i file non elaborati di CSS quando si esegue il `Development` ambiente:

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=21-24)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=9-12)]

::: moniker-end

Quanto segue `environment` tag esegue il rendering di file CSS in bundle e minimizzati durante l'esecuzione in un ambiente diverso da `Development`. Ad esempio, in esecuzione in `Production` o `Staging` attiva il rendering di tali fogli di stile:

::: moniker range=">= aspnetcore-2.0"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=5&range=25-30)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-cshtml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/Pages/_Layout.cshtml?highlight=3&range=13-18)]

::: moniker-end

## <a name="consume-bundleconfigjson-from-gulp"></a>Utilizzare bundleconfig.json da Gulp

Vi sono casi in cui e minimizzazione flusso di lavoro un'app richiede un'ulteriore elaborazione. Sono esempi di ottimizzazione dell'immagine, cache busting ed elaborazione di asset della rete CDN. Per soddisfare questi requisiti, è possibile convertire il flusso di lavoro di creazione di bundle e minimizzazione per l'uso di Gulp.

### <a name="use-the-bundler--minifier-extension"></a>Usare l'estensione di Bundler & Minifier

Visual Studio [Bundler & Minifier](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.BundlerMinifier) estensione gestisce la conversione a Gulp.

> [!NOTE]
> L'estensione Bundler & Minifier appartiene a un progetto in GitHub per il quale Microsoft non fornisce supporto basato sulla community. Problemi devono essere segnalati [qui](https://github.com/madskristensen/BundlerMinifier/issues).

Fare doppio clic il *bundleconfig.json* del file in Esplora soluzioni e selezionare **Bundler & Minifier** > **convertire a Gulp...** :

![Convertire a Gulp menu di scelta rapida](../client-side/bundling-and-minification/_static/convert-to-gulp.png)

Il *gulpfile. js* e *package. JSON* file vengono aggiunti al progetto. Il tipo di supporto [npm](https://www.npmjs.com/) i pacchetti elencati nel *package. JSON* del file `devDependencies` sezione installate.

Eseguire il comando seguente nella finestra della console di gestione pacchetti per installare l'interfaccia della riga di comando Gulp come dipendenza globale:

```console
npm i -g gulp-cli
```

Il *gulpfile. js* letture di file le *bundleconfig.json* file per l'input, output e le impostazioni.

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-12&highlight=10)]

### <a name="convert-manually"></a>Convertire manualmente

Se Visual Studio e/o l'estensione Bundler & Minifier non è disponibile, convertire manualmente.

Aggiungere un *package. JSON* file, con il codice seguente `devDependencies`, alla radice del progetto:

[!code-json[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/package.json?range=5-13)]

Installare le dipendenze eseguendo il comando seguente allo stesso livello *package. JSON*:

```console
npm i
```

Installare l'interfaccia della riga di comando Gulp come dipendenza globale:

```console
npm i -g gulp-cli
```

Copia il *gulpfile. js* file sotto la radice del progetto:

[!code-javascript[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/gulpfile.js?range=1-11,14-)]

### <a name="run-gulp-tasks"></a>Eseguire le attività Gulp

Per attivare l'attività di minimizzazione Gulp prima il progetto viene compilato in Visual Studio, aggiungere quanto segue [destinazione MSBuild](/visualstudio/msbuild/msbuild-targets) al file *. csproj:

[!code-xml[](../client-side/bundling-and-minification/samples/BuildBundlerMinifierApp/BuildBundlerMinifierApp.csproj?range=14-16)]

In questo esempio, tutte le attività definiti all'interno di `MyPreCompileTarget` destinazione eseguire prima dell'oggetto predefinito `Build` destinazione. Nella finestra di Output di Visual Studio viene visualizzato output simile al seguente:

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

In alternativa, Visual Studio Task Runner Explorer può essere utilizzato per associare le attività Gulp a eventi specifici di Visual Studio. Visualizzare [esecuzione di attività predefinito](xref:client-side/using-gulp#running-default-tasks) per istruzioni su come questa operazione.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Usare Gulp](xref:client-side/using-gulp)
* [Usare Grunt](xref:client-side/using-grunt)
* [Usare più ambienti](xref:fundamentals/environments)
* [Helper tag](xref:mvc/views/tag-helpers/intro)
