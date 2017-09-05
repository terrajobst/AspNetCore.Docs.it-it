---
title: Come aggregare e riduzione in ASP.NET Core
author: spboyer
description: 
keywords: ASP.NET Core, aggregazione e minimizzazione CSS, JavaScript, minimizzare, BuildBundlerMinifier
ms.author: riande
manager: wpickett
ms.date: 02/28/2017
ms.topic: article
ms.assetid: d54230f9-8e5f-4861-a29c-1d3a14e0b0d9
ms.technology: aspnet
ms.prod: aspnet-core
uid: client-side/bundling-and-minification
ms.openlocfilehash: 810d89798330aeb1c387ec85eb05b1f4efde167a
ms.sourcegitcommit: 275a5381b6172b4f0b5fcd1d252aff03d3dae166
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/30/2017
---
# <a name="bundling-and-minification-in-aspnet-core"></a>Come aggregare e riduzione in ASP.NET Core

Creazione di bundle e minimizzazione sono due tecniche è possibile utilizzare in ASP.NET per migliorare le prestazioni di caricamento di pagina per l'applicazione web. Creazione di bundle combina più file in un singolo file. Minimizzazione esegue diverse ottimizzazioni di codice diverse per gli script e CSS, determinando di payload più piccoli. Utilizzati insieme, aggregazione e minimizzazione migliora le prestazioni in fase di carico riducendo il numero di richieste al server e riduzione delle dimensioni degli asset richiesti (ad esempio file CSS e JavaScript).

In questo articolo vengono illustrati i vantaggi dell'utilizzo di aggregazione e riduzione, incluso come queste funzionalità possono essere utilizzate con le applicazioni ASP.NET Core.

## <a name="overview"></a>Panoramica

Nelle applicazioni ASP.NET Core, esistono diverse opzioni per l'aggregazione e la riduzione delle risorse sul lato client. I modelli di core per MVC forniscono una soluzione di casella utilizzando un file di configurazione e il pacchetto BuildBundlerMinifier NuGet. Strumenti di terze parti, ad esempio [Gulp](using-gulp.md) e [Grunt](using-grunt.md) sono disponibili anche per eseguire le stesse attività dei processi richiede ulteriore flusso di lavoro o di complessità. Usando come aggregare in fase di progettazione e la riduzione, vengono creati i file minimizzati prima della distribuzione dell'applicazione. Aggiunta e la riduzione prima della distribuzione offre il vantaggio del carico del server ridotto. Tuttavia, è importante tenere presente che in fase di progettazione di aggregazione e minimizzazione aumenta la complessità di compilazione e funziona solo con file statici.

Come aggregare e minimizzazione principalmente migliorare il tempo di caricamento di prima pagina richiesta. Una volta che una pagina web è stato richiesto, il browser memorizza nella cache le risorse (JavaScript, CSS e immagini) e aggregazione e riduzione non forniscono alcun miglioramento delle prestazioni quando si richiede la stessa pagina oppure pagine nello stesso sito richiede le stesse risorse. Se non si imposta la scadenza testata correttamente le risorse e non si utilizza come aggregare e minimizzazione, euristica di aggiornamento del browser contrassegnerà l'asset non aggiornati dopo alcuni giorni e il browser richiederà una richiesta di convalida per ogni asset. In questo caso, aggregazione e minimizzazione forniscono un aumento delle prestazioni anche dopo la prima richiesta di pagina.

### <a name="bundling"></a>Creazione di bundle

Creazione di bundle è una funzionalità che rende più semplice combinare o aggregare più file in un singolo file. Poiché l'aggregazione combina più file in un singolo file, riduce il numero di richieste al server in cui sono necessari per recuperare e visualizzare una risorsa web, ad esempio una pagina web. È possibile creare CSS, JavaScript e altri pacchetti. Un numero inferiore di file significa meno le richieste HTTP dal browser al server o dal servizio fornendo all'applicazione. I risultati miglioramento delle prestazioni di caricamento prima pagina.

### <a name="minification"></a>Minimizzazione

Riduzione di eseguire una serie di ottimizzazioni di codice diverso per ridurre le dimensioni di un asset richiesti (ad esempio, CSS, immagini, file JavaScript). Risultati comuni della minimizzazione includono la rimozione di spazi vuoti non necessari e i commenti e abbreviare i nomi delle variabili per un carattere.

Si consideri la funzione JavaScript seguente:

```javascript
AddAltToImg = function (imageTagAndImageID, imageContext) {
  ///<signature>
  ///<summary> Adds an alt tab to the image
  // </summary>
  //<param name="imgElement" type="String">The image selector.</param>
  //<param name="ContextForImage" type="String">The image context.</param>
  ///</signature>
  var imageElement = $(imageTagAndImageID, imageContext);
  imageElement.attr('alt', imageElement.attr('id').replace(/ID/, ''));
}
```

Dopo la minimizzazione, la funzione viene ridotto al seguente:

```javascript
AddAltToImg=function(t,a){var r=$(t,a);r.attr("alt",r.attr("id").replace(/ID/,""))};
```

Oltre a rimuovere i commenti e gli spazi vuoti non necessari, i seguenti parametri e i nomi delle variabili sono stati rinominati (abbreviato) come indicato di seguito:

Originale | Ridenominazione
--- | :---:
imageTagAndImageID | u
imageContext | a
imageElement | f

## <a name="impact-of-bundling-and-minification"></a>Impatto di aggregazione e di riduzione

La tabella seguente illustra alcune importanti differenze tra l'elenco di tutte le risorse singolarmente e con aggregazione e la riduzione in una semplice pagina web:

Azione | Con B/M | Senza B/M | Modifica
--- | :---: | :---: | :---:
Richieste di file |7 | 18 | 157%
KB trasferiti | 156 | 264.68 | 70%
Tempo di caricamento (MS) | 885 | 2360 | 167%

I byte inviati aveva una significativa riduzione con aggregazione come browser sono piuttosto dettagliati con le intestazioni HTTP che si applicano alle richieste. Il tempo di caricamento Mostra un miglioramento grande, ma in questo esempio è stato eseguito localmente. Si otterrà un aumento delle prestazioni durante l'utilizzo di aggregazione e minimizzazione asset trasferiti su una rete.

## <a name="using-bundling-and-minification-in-a-project"></a>Utilizzo di aggregazione e riduzione in un progetto

Il modello di progetto MVC fornisce un `bundleconfig.json` file di configurazione che definisce le opzioni per ogni pacchetto. Per impostazione predefinita, viene modificata una configurazione di pacchetto per il codice JavaScript personalizzato (`wwwroot/js/site.js`) e fogli di stile (`wwwroot/css/site.css`) file.

[!code-json[Principale](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig.json)]

Opzioni di raggruppamento includono:

* outputFileName - nome del file di bundle all'output. Può contenere un percorso relativo di `bundleconfig.json` file. **Obbligatorio**
* inputFiles - matrice di file da raggruppare. Questi sono i percorsi relativi al file di configurazione. **parametro facoltativo**, * comporta un valore vuoto in un file di output vuoto. [il glob](http://www.tldp.org/LDP/abs/html/globbingref.html) sono supportati i modelli.
* minimizzare - minimizzazione opzioni per l'output di tipo. **parametro facoltativo**, *impostazione predefinita:`minify: { enabled: true }`*
  * Opzioni di configurazione sono disponibili per ogni tipo di file di output.
    * [Minimizzatore CSS](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [Minimizzatore JavaScript](https://github.com/madskristensen/BundlerMinifier/wiki/jsminifier)
    * [Minimizzatore HTML](https://github.com/madskristensen/BundlerMinifier/wiki/htmlminifier)
* includeInProject - aggiungere i file generati file di progetto. **parametro facoltativo**, *predefinito: false*
* mapping di origine - Genera mapping di origine per il file aggregato. **parametro facoltativo**, *predefinito: false*

### <a name="visual-studio-2015--2017"></a>Visual Studio 2015 / 2017

Aprire `bundleconfig.json` in Visual Studio, se l'ambiente non ha l'estensione installato; in un prompt dei comandi viene presentato suggerendo che sia presente una che potrebbe consentire a questo tipo di file.

![Estensione BuildBundlerMinifier suggerimento](../client-side/bundling-and-minification/_static/bundler-extension-suggestion.png)

Visualizzare le estensioni di selezionare e installare il **Bundler & Minimizzatore** estensione (riavviare richiede Visual Studio).

![Estensione BuildBundlerMinifier suggerimento](../client-side/bundling-and-minification/_static/view-extension.png)

Una volta completato il riavvio, è necessario configurare la compilazione per eseguire i processi di scena e creazione di bundle di risorse sul lato client. Fare doppio clic su di `bundleconfig.json` file e selezionare *Enable bundle in fase di compilazione...* .

Compilare il progetto e `bundleconfig.json` è incluso nel processo di compilazione per generare i file di output in base alla configurazione.

```console
1>------ Build started: Project: BuildBundlerMinifierExample, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierExample -> C:\BuildBundlerMinifierExample\bin\Debug\netcoreapp1.1\BuildBundlerMinifierExample.dll
========== Build: 1 succeeded or up-to-date, 0 failed, 0 skipped ==========
```

### <a name="visual-studio-code-or-command-line"></a>Codice di Visual Studio o di riga di comando

Visual Studio e l'estensione guidare il processo di creazione di bundle e riduzione mediante i movimenti GUI; Tuttavia, le stesse funzionalità sono disponibili con il `dotnet` pacchetto CLI e BuildBundlerMinifier NuGet.

Aggiungere il pacchetto NuGet al progetto:

```console
dotnet add package BuildBundlerMinifier
```

Ripristinare le dipendenze:

```console
dotnet restore
```

Compilare l'applicazione:

```console
dotnet build
```

I risultati dell'aggregazione in base a quello configurato e/o la minimizzazione illustrato nell'output del comando di compilazione.

```console
Microsoft (R) Build Engine version 15.1.545.13942
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Begin processing bundleconfig.json
     Minified wwwroot/css/site.min.css
  Bundler: Done processing bundleconfig.json
  BuildBundlerMinifierExample -> /BuildBundlerMinifierExample/bin/Debug/netcoreapp1.0/BuildBundlerMinifierExample.dll
```

## <a name="adding-files"></a>Aggiunta di file

In questo esempio, un file CSS aggiuntivo viene aggiunto chiamato `custom.css` e configurato per la creazione di bundle e minimizzazione con `site.css`, risultante in un singolo `site.min.css`.

Custom.CSS

```css
.about, [role=main], [role=complementary]
{
    margin-top: 60px;
}

footer
{
    margin-top: 10px;
}
```

Aggiungere il relativo percorso `bundleconfig.json`.

[!code-json[Principale](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig2.json)]

> [!NOTE]
> In alternativa, è Impossibile utilizzare il modello il glob - `"inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]` che ottiene CSS tutti i file e consente di escludere il modello di file minimizzata.

Compilare l'applicazione e se si apre `site.min.css`, si noterà ora che contenuto del `custom.css` è stato aggiunto alla fine del file.

## <a name="controlling-bundling-and-minification"></a>Controllo di aggregazione e riduzione

In generale, si desidera utilizzare i file in bundle e minimizzati dell'app solo in un ambiente di produzione. Durante lo sviluppo, si desidera utilizzare i file originali, pertanto è più semplice eseguire il debug dell'app.

È possibile specificare quali script e file CSS da includere nelle pagine utilizzando l'helper di tag di ambiente nelle pagine di layout (vedere [gli helper di Tag](../mvc/views/tag-helpers/index.md)). L'helper di tag di ambiente eseguirà il rendering del relativo contenuto solo quando è in esecuzione in ambienti specifici. Vedere [utilizzo di più ambienti](../fundamentals/environments.md) per informazioni dettagliate su come specificare l'ambiente corrente.

Il seguente tag di ambiente verrà eseguito il rendering di file CSS non elaborati durante l'esecuzione nel `Development` ambiente:

[!code-html[Principale](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=3&range=9-12)]

Il tag di ambiente verrà eseguito il rendering di file CSS in dotazione e minimizzati solo durante l'esecuzione in `Production` o `Staging`:

[!code-html[Principale](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=5&range=13-18)]

## <a name="consuming-bundleconfigjson-from-gulp"></a>Utilizzo di bundleconfig.json da Gulp

Se il flusso di lavoro come aggregare e riduzione del app richiede procedure aggiuntive, ad esempio l'elaborazione di immagini, busting cache, l'elaborazione di rete CDN assest, e così via, è possibile convertire il processo di raggruppamento e Minify a Gulp.

> [!NOTE]
> Opzione di conversione disponibile solo in Visual Studio 2015 e 2017.

Fare doppio clic su di `bundleconfig.json` e selezionare **convertire Gulp...** . Verrà generato il `gulpfile.js` e installare i pacchetti necessari npm.

![Convertire Gulp](../client-side/bundling-and-minification/_static/convert-togulp.png)

Il `gulpfile.js` prodotti letture di `bundleconfig.json` file per la configurazione, pertanto può continuare a essere usata per l'input/output e le impostazioni.

[!code-javascript[Principale](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/gulpfile.js)]

Per abilitare Gulp quando si compila il progetto in Visual Studio 2017, aggiungere il file *. csproj quanto segue:

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
    <Exec Command="gulp min" />
</Target>
```

Per abilitare Gulp quando si compila il progetto in Visual Studio 2015, aggiungere il comando seguente per il `project.json` file:

```json
"scripts": {
    "precompile": "gulp min"
}
```

## <a name="additional-resources"></a>Risorse aggiuntive

* [Uso di Gulp](using-gulp.md)
* [Utilizzo di Grunt](using-grunt.md)
* [Utilizzo di più ambienti](../fundamentals/environments.md)
* [Helper di tag](../mvc/views/tag-helpers/index.md)
