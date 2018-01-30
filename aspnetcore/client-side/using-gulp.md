---
title: Uso di Gulp in ASP.NET Core
author: rick-anderson
description: Informazioni su come utilizzare Gulp in ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/using-gulp
ms.openlocfilehash: f091370bc85a37eeaac1291a2fdc6ea85164f148
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-using-gulp-in-aspnet-core"></a>Introduzione all'uso di Gulp in ASP.NET Core 

Da [Erik Reitan](https://github.com/Erikre), [Scott Addie](https://scottaddie.com), [Daniel Roth](https://github.com/danroth27), e [Shayne Boyer](https://twitter.com/spboyer)

In un'app web moderna tipiche, potrebbe essere il processo di compilazione:

* Aggregare e minimizzare file CSS e JavaScript.
* Eseguire gli strumenti per chiamare le operazioni di aggregazione e minimizzazione prima di ogni compilazione.
* Compilare meno o SASS file CSS.
* Compilare i file di CoffeeScript o TypeScript a JavaScript.

Oggetto *runner attività* è uno strumento che consente di automatizzare le attività di sviluppo di routine e altro ancora. Visual Studio fornisce supporto incorporato per due canali di comuni attività basate su JavaScript: [Gulp](https://gulpjs.com/) e [Grunt](using-grunt.md).

## <a name="gulp"></a>gulp

Gulp è un basate su JavaScript streaming compilazione toolkit per il codice sul lato client. Viene usata in genere per i file sul lato client tramite una serie di processi di flusso quando viene generato un evento specifico in un ambiente di compilazione. Ad esempio, Gulp possono essere utilizzati per automatizzare [come aggregare e minimizzazione](bundling-and-minification.md) o la pulizia di un ambiente di sviluppo prima di una nuova compilazione.

Viene definito un set di attività Gulp in *gulpfile.js*. Il codice JavaScript seguente include i moduli Gulp e specifica i percorsi di file per fare riferimento all'interno delle attività imminente:

```javascript
/// <binding Clean='clean' />
"use strict";

var gulp = require("gulp"),
  rimraf = require("rimraf"),
  concat = require("gulp-concat"),
  cssmin = require("gulp-cssmin"),
  uglify = require("gulp-uglify");

var paths = {
  webroot: "./wwwroot/"
};

paths.js = paths.webroot + "js/**/*.js";
paths.minJs = paths.webroot + "js/**/*.min.js";
paths.css = paths.webroot + "css/**/*.css";
paths.minCss = paths.webroot + "css/**/*.min.css";
paths.concatJsDest = paths.webroot + "js/site.min.js";
paths.concatCssDest = paths.webroot + "css/site.min.css";
```

Il codice sopra riportato specifica i moduli di nodo sono necessari. Il `require` ogni modulo importazioni di funzioni in modo che le attività dipendenti possono utilizzare le funzionalità incluse. Ciascuno dei moduli importati viene assegnato a una variabile. I moduli possono essere situati in base al nome o percorso. In questo esempio, i moduli denominato `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, e `gulp-uglify` vengono recuperati in base al nome. Inoltre, una serie di percorsi vengono creati in modo che i percorsi di file CSS e JavaScript possono essere riutilizzati e a cui fa riferimento all'interno delle attività. Nella tabella seguente vengono fornite le descrizioni dei moduli inclusi in *gulpfile.js*.

| Nome modulo | Descrizione |
| ----------- | ----------- |
| gulp        | Il sistema di compilazione flusso Gulp. Per ulteriori informazioni, vedere [gulp](https://www.npmjs.com/package/gulp). |
| rimraf      | Un modulo di eliminazione del nodo. Per ulteriori informazioni, vedere [rimraf](https://www.npmjs.com/package/rimraf). |
| gulp concat | Un modulo che concatena i file in base al carattere di nuova riga del sistema operativo. Per ulteriori informazioni, vedere [gulp concat](https://www.npmjs.com/package/gulp-concat). |
| gulp-cssmin | Un modulo che minimizza file CSS. Per ulteriori informazioni, vedere [gulp cssmin](https://www.npmjs.com/package/gulp-cssmin). |
| uglify gulp | Un modulo che minimizza *. js* file. Per ulteriori informazioni, vedere [uglify gulp](https://www.npmjs.com/package/gulp-uglify). |

Dopo aver importati i moduli necessari, è possibile specificare le attività. Di seguito sono riportate le sei attività registrato, rappresentato dal codice seguente:

```javascript
gulp.task("clean:js", function (cb) {
  rimraf(paths.concatJsDest, cb);
});

gulp.task("clean:css", function (cb) {
  rimraf(paths.concatCssDest, cb);
});

gulp.task("clean", ["clean:js", "clean:css"]);

gulp.task("min:js", function () {
  return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
    .pipe(concat(paths.concatJsDest))
    .pipe(uglify())
    .pipe(gulp.dest("."));
});

gulp.task("min:css", function () {
  return gulp.src([paths.css, "!" + paths.minCss])
    .pipe(concat(paths.concatCssDest))
    .pipe(cssmin())
    .pipe(gulp.dest("."));
});

gulp.task("min", ["min:js", "min:css"]);
```

Nella tabella seguente viene fornita una spiegazione delle attività specificato nel codice precedente:

|Nome attività|Descrizione|
|--- |--- |
|Pulisci: js|Attività che usa il modulo di eliminazione del nodo rimraf per rimuovere la versione del file site.js minimizzata.|
|clean:css|Attività che usa il modulo di eliminazione del nodo rimraf per rimuovere la versione del file site.css minimizzata.|
|Pulire|Un'attività che chiama il `clean:js` attività, aggiungendo il `clean:css` attività.|
|min:js|Attività che minimizza e concatena tutti i file con estensione js all'interno della cartella js. I. min.js i file vengono esclusi.|
|min:css|Attività che minimizza e concatena tutti i file con estensione CSS all'interno della cartella di css. I. min.css i file vengono esclusi.|
|min|Un'attività che chiama il `min:js` attività, aggiungendo il `min:css` attività.|

## <a name="running-default-tasks"></a>Esecuzione di attività predefinito

Se è già stata creata una nuova app Web, creare un nuovo progetto applicazione Web ASP.NET in Visual Studio.

1.  Aggiungere un nuovo file JavaScript al progetto e denominarlo *gulpfile.js*, quindi copiare il codice seguente.

    ```javascript
    /// <binding Clean='clean' />
    "use strict";
    
    var gulp = require("gulp"),
      rimraf = require("rimraf"),
      concat = require("gulp-concat"),
      cssmin = require("gulp-cssmin"),
      uglify = require("gulp-uglify");
    
    var paths = {
      webroot: "./wwwroot/"
    };
    
    paths.js = paths.webroot + "js/**/*.js";
    paths.minJs = paths.webroot + "js/**/*.min.js";
    paths.css = paths.webroot + "css/**/*.css";
    paths.minCss = paths.webroot + "css/**/*.min.css";
    paths.concatJsDest = paths.webroot + "js/site.min.js";
    paths.concatCssDest = paths.webroot + "css/site.min.css";
    
    gulp.task("clean:js", function (cb) {
      rimraf(paths.concatJsDest, cb);
    });
    
    gulp.task("clean:css", function (cb) {
      rimraf(paths.concatCssDest, cb);
    });
    
    gulp.task("clean", ["clean:js", "clean:css"]);
    
    gulp.task("min:js", function () {
      return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
        .pipe(concat(paths.concatJsDest))
        .pipe(uglify())
        .pipe(gulp.dest("."));
    });
    
    gulp.task("min:css", function () {
      return gulp.src([paths.css, "!" + paths.minCss])
        .pipe(concat(paths.concatCssDest))
        .pipe(cssmin())
        .pipe(gulp.dest("."));
    });
    
    gulp.task("min", ["min:js", "min:css"]);
    ```

2.  Aprire il *package. JSON* file (aggiungere se non esiste) e aggiungere il seguente.

    ```json
    {
      "devDependencies": {
        "gulp": "3.9.1",
        "gulp-concat": "2.6.1",
        "gulp-cssmin": "0.1.7",
        "gulp-uglify": "2.0.1",
        "rimraf": "2.6.1"
      }
    }
    ```

3.  In **Esplora**, fare doppio clic su *gulpfile.js*e selezionare **Task Runner Explorer**.
    
    ![Aprire Task Runner Explorer da Esplora soluzioni](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    **Esplora esecuzione attività** Mostra un elenco di attività Gulp. (Potrebbe essere necessario fare clic su di **aggiornamento** pulsante visualizzato a sinistra del nome del progetto.)
    
    ![Esplora esecuzione attività](using-gulp/_static/03-TaskRunnerExplorer.png)

4.  Trova di sotto **attività** in **Task Runner Explorer**, fare doppio clic su **pulita**e selezionare **eseguire** dal menu a comparsa.

    ![Attività di pulizia dell'Esplora esecuzione attività](using-gulp/_static/04-TaskRunner-clean.png)

    **Esplora esecuzione attività** creerà una nuova scheda denominata **pulita** ed eseguire l'attività clean come definito *gulpfile.js*.

5.  Fare doppio clic su di **pulita** attività, quindi selezionare **associazioni** > **prima di compilare**.

    ![Associazione BeforeBuild Task Runner Explorer](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    Il **prima di compilare** associazione consente di configurare l'attività clean per l'esecuzione automatica prima di ogni compilazione del progetto.

Le associazioni è impostare con **Task Runner Explorer** vengono archiviate in forma di un commento nella parte superiore del *gulpfile.js* e diventano effettive solo in Visual Studio. Alternativa che non richiede Visual Studio, è possibile configurare l'esecuzione automatica di attività gulp il *csproj* file. Ad esempio, contestualizzare il *csproj* file:

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

Ora l'attività di pulizia viene eseguito quando si esegue il progetto in Visual Studio o da un prompt dei comandi tramite il `dotnet run` comando (eseguire `npm install` prima).

## <a name="defining-and-running-a-new-task"></a>Definizione e l'esecuzione di una nuova attività

Per definire una nuova attività Gulp, modificare *gulpfile.js*.

1.  Aggiungere il codice JavaScript seguente alla fine di *gulpfile.js*:

    ```javascript
    gulp.task("first", function () {
      console.log('first task! <-----');
    });
    ```

    Questa operazione è denominata `first`, e visualizza semplicemente una stringa.

2.  Salvare *gulpfile.js*.

3.  In **Esplora**, fare doppio clic su *gulpfile.js*e selezionare *Task Runner Explorer*.

4.  In **Task Runner Explorer**, fare doppio clic su **prima**e selezionare **eseguire**.

    ![Eseguire innanzitutto Task Runner Explorer](using-gulp/_static/06-TaskRunner-First.png)

    Si noterà che viene visualizzato il testo di output. Se si è interessati negli esempi in base a uno scenario comune, vedere le recipe Gulp.

## <a name="defining-and-running-tasks-in-a-series"></a>Definizione e l'esecuzione di attività in una serie

Quando si eseguono più attività, le attività eseguite contemporaneamente per impostazione predefinita. Tuttavia, se è necessario eseguire le attività in un ordine specifico, è necessario specificare quando ogni attività sono stata completata, nonché come quali attività dipendono dal completamento di un'altra attività.

1.  Per definire una serie di attività da eseguire in ordine, sostituire il `first` attività aggiunte in precedenza nella *gulpfile.js* con quanto segue:

    ```javascript
    gulp.task("series:first", function () {
      console.log('first task! <-----');
    });
 
    gulp.task("series:second", ["series:first"], function () {
      console.log('second task! <-----');
    });
 
    gulp.task("series", ["series:first", "series:second"], function () {});
    ```
 
    Ora è tre attività: `series:first`, `series:second`, e `series`. Il `series:second` attività include un secondo parametro che specifica una matrice di attività deve essere eseguita e completata prima di `series:second` attività verrà eseguita. Come specificato nel codice precedente, solo il `series:first` attività deve essere completata prima di `series:second` attività verrà eseguita.

2.  Salvare *gulpfile.js*.

3.  In **Esplora**, fare doppio clic su *gulpfile.js* e selezionare **Task Runner Explorer** se non è già aperto.

4.  In **Task Runner Explorer**, fare doppio clic su **serie** e selezionare **eseguire**.

    ![Eseguire attività di serie Task Runner Explorer](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a>IntelliSense

IntelliSense offre il completamento del codice, descrizioni dei parametri e altre funzionalità per la produttività e ridurre gli errori. Attività gulp scritte in JavaScript. Pertanto, IntelliSense può fornire assistenza durante lo sviluppo. Mentre si lavora con JavaScript, IntelliSense Elenca gli oggetti, funzioni, proprietà e i parametri che sono disponibili in base al contesto corrente. Selezionare un'opzione di codifica dall'elenco popup fornito da IntelliSense per completare il codice.

![gulp IntelliSense](using-gulp/_static/08-IntelliSense.png)

Per ulteriori informazioni su IntelliSense, vedere [JavaScript IntelliSense](https://docs.microsoft.com/visualstudio/ide/javascript-intellisense).

## <a name="development-staging-and-production-environments"></a>Ambienti di sviluppo, gestione temporanea e produzione

Quando Gulp viene utilizzata per ottimizzare i file sul lato client per la gestione temporanea e produzione, i file elaborati vengono salvati in un percorso locale di gestione temporanea e produzione. Il *layout. cshtml* file Usa il **ambiente** tag helper per fornire due diverse versioni di file CSS. È una versione di file CSS per lo sviluppo e l'altra versione è ottimizzata per la gestione temporanea e produzione. In Visual Studio 2017, quando si modifica il **ASPNETCORE_ENVIRONMENT** variabile di ambiente `Production`, Visual Studio verrà compilato l'app Web e un collegamento ai file CSS ridotta a icona. Il markup seguente mostra il **ambiente** gli helper contenente il tag di collegamento al tag di `Development` CSS file e il minimizzata `Staging, Production` file CSS.

```html
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
    <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
    <script src="~/js/site.js" asp-append-version="true"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.2.0.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery"
            crossorigin="anonymous"
            integrity="sha384-K+ctZQ+LL8q6tP7I94W+qzQsfRV2a+AfHIi9k8z8l9ggpc8X+Ytst4yBo/hH+8Fk">
    </script>
    <script src="https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js"
            asp-fallback-src="~/lib/bootstrap/dist/js/bootstrap.min.js"
            asp-fallback-test="window.jQuery && window.jQuery.fn && window.jQuery.fn.modal"
            crossorigin="anonymous"
            integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa">
    </script>
    <script src="~/js/site.min.js" asp-append-version="true"></script>
</environment>
```

## <a name="switching-between-environments"></a>Il passaggio tra ambienti

Per passare tra la compilazione per ambienti diversi, modificare il **ASPNETCORE_ENVIRONMENT** valore della variabile di ambiente.

1.  In **Task Runner Explorer**, verificare che il **min** attività è stata impostata per l'esecuzione **prima di compilare**.

2.  In **Esplora**, il nome del progetto e scegliere **proprietà**.

    Viene visualizzata la finestra delle proprietà per l'app Web.

3.  Fare clic sulla scheda **Debug**.

4.  Impostare il valore della **: l'ambiente di Hosting** variabile di ambiente `Production`.

5.  Premere **F5** per eseguire l'applicazione in un browser.

6.  Nella finestra del browser, la pagina e scegliere **Visualizza origine** per visualizzare il codice HTML della pagina.

    Si noti che i collegamenti di foglio di stile puntano ai file CSS minimizzati.

7.  Chiudere il browser per arrestare l'applicazione Web.

8.  In Visual Studio, tornare alla finestra delle proprietà per l'app Web e modificare il **: l'ambiente di Hosting** variabile di ambiente nuovamente a `Development`.

9.  Premere **F5** a eseguire nuovamente l'applicazione in un browser.

10. Nella finestra del browser, la pagina e scegliere **Visualizza origine** per visualizzare il codice HTML della pagina.

    Si noti che i collegamenti di foglio di stile puntano alle versioni dei file CSS non minimizzate.

Per ulteriori informazioni relative agli ambienti di ASP.NET Core, vedere [utilizzo di più ambienti](../fundamentals/environments.md).

## <a name="task-and-module-details"></a>Dettagli attività e il modulo

Un'attività Gulp è registrata con un nome di funzione. Se è necessario eseguire altre attività prima dell'attività corrente, è possibile specificare le dipendenze. Funzioni aggiuntive consentono di eseguire e controllare le attività Gulp, nonché di impostare l'origine (*src*) e di destinazione (*dest*) dei file da modificare. Le funzioni API Gulp principali sono i seguenti:

|Gulp (funzione)|Sintassi|Descrizione|
|---   |--- |--- |
|attività  |`gulp.task(name[, deps], fn) { }`|Il `task` funzione crea un'attività. Il `name` parametro definisce il nome dell'attività. Il `deps` parametro contiene una matrice di attività da completare prima dell'esecuzione di questa attività. Il `fn` parametro rappresenta una funzione di callback che esegue le operazioni dell'attività.|
|Espressioni di controllo |`gulp.watch(glob [, opts], tasks) { }`|Il `watch` funzione consente di monitorare le attività di file e viene eseguito quando si verifica una modifica di file. Il `glob` parametro è un `string` o `array` che determina quali sono i file da controllare. Il `opts` parametro fornisce controllo Opzioni aggiuntive del file.|
|src   |`gulp.src(globs[, options]) { }`|Il `src` funzione fornisce i file che corrispondono ai valori glob. Il `glob` parametro è un `string` o `array` che determina quali sono i file da leggere. Il `options` parametro fornisce opzioni aggiuntive del file.|
|dest  |`gulp.dest(path[, options]) { }`|Il `dest` funzione definisce una posizione in cui è possano scrivere file. Il `path` parametro è una stringa o una funzione che determina la cartella di destinazione. Il `options` parametro è un oggetto che specifica le opzioni di cartella di output.|

Per ulteriori informazioni di riferimento Gulp API, vedere [Gulp documenti API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).

## <a name="gulp-recipes"></a>Gulp ricette

La Comunità Gulp fornisce Gulp [ricette](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md). Queste soluzioni sono costituiti da attività Gulp per scenari comuni.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Documentazione gulp](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [Come aggregare e riduzione in ASP.NET Core](bundling-and-minification.md)
* [Utilizzo di Grunt in ASP.NET Core](using-grunt.md)
