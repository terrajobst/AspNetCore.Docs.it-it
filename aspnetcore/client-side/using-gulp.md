---
title: Usare Gulp in ASP.NET Core
author: rick-anderson
description: Informazioni su come usare Gulp in ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/04/2018
uid: client-side/using-gulp
ms.openlocfilehash: 9f6d03a1e8a81bceca15cb1e1aa664c22c31e1d3
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209872"
---
# <a name="use-gulp-in-aspnet-core"></a>Usare Gulp in ASP.NET Core

Dal [Scott Addie](https://scottaddie.com), [Shayne Boyer](https://twitter.com/spboyer), e [pino David](https://twitter.com/davidpine7)

In un'app web moderne tipico, potrebbe essere il processo di compilazione:

* Aggregare e minimizzare file CSS e JavaScript.
* Eseguire gli strumenti per chiamare le operazioni di creazione di bundle e minimizzazione prima di ogni compilazione.
* Compilare meno o il SASS file CSS.
* Compilare i file TypeScript o CoffeeScript a JavaScript.

Oggetto *strumento di esecuzione attività* è uno strumento che consente di automatizzare le attività di sviluppo di routine e altro ancora. Visual Studio fornisce supporto incorporato per gli strumenti di esecuzione di due attività basate su JavaScript più diffusi: [Gulp](https://gulpjs.com/) e [Grunt](using-grunt.md).

## <a name="gulp"></a>Gulp

Gulp è un basate su JavaScript streaming compilazione toolkit per il codice lato client. Viene comunemente utilizzato per trasmettere file lato client tramite una serie di processi quando viene generato un evento specifico in un ambiente di compilazione. Ad esempio, Gulp può essere utilizzato per automatizzare [creazione di bundle e minimizzazione](bundling-and-minification.md) o la pulizia di un ambiente di sviluppo prima di una nuova compilazione.

È definito un set di attività Gulp *gulpfile. js*. Il codice JavaScript seguente include i moduli di Gulp e specifica i percorsi di file a cui fare riferimento all'interno delle attività future:

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

Il codice sopra riportato specifica che i moduli del nodo sono necessari. Il `require` ogni modulo importazioni di funzioni in modo che le attività dipendenti possono usare le funzionalità incluse. Ognuno dei moduli importati viene assegnato a una variabile. I moduli possono trovarsi in base al nome o percorso. In questo esempio, i moduli denominati `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, e `gulp-uglify` vengono recuperati in base al nome. Inoltre, una serie di tracciati vengono creati in modo che i percorsi dei file CSS e JavaScript possono essere riutilizzati e fa riferimento all'interno delle attività. Nella tabella seguente fornisce le descrizioni dei moduli inclusi in *gulpfile. js*.

| Nome modulo | Descrizione |
| ----------- | ----------- |
| gulp        | Il sistema di compilazione streaming Gulp. Per altre informazioni, vedere [gulp](https://www.npmjs.com/package/gulp). |
| rimraf      | Un modulo di eliminazione del nodo. Per altre informazioni, vedere [rimraf](https://www.npmjs.com/package/rimraf). |
| gulp-concat | Un modulo che concatena i file in base al carattere di nuova riga del sistema operativo. Per altre informazioni, vedere [gulp concat](https://www.npmjs.com/package/gulp-concat). |
| gulp-cssmin | Un modulo che minimizza file CSS. Per altre informazioni, vedere [gulp cssmin](https://www.npmjs.com/package/gulp-cssmin). |
| gulp-uglify | Un modulo che minimizza *js* file. Per altre informazioni, vedere [uglify gulp](https://www.npmjs.com/package/gulp-uglify). |

Dopo aver importati i moduli richiesti, è possibile specificare le attività. Di seguito sono presenti sei attività registrate, rappresentata nel codice seguente:

```javascript
gulp.task("clean:js", done => rimraf(paths.concatJsDest, done));
gulp.task("clean:css", done => rimraf(paths.concatCssDest, done));
gulp.task("clean", gulp.series(["clean:js", "clean:css"]));

gulp.task("min:js", () => {
  return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
    .pipe(concat(paths.concatJsDest))
    .pipe(uglify())
    .pipe(gulp.dest("."));
});

gulp.task("min:css", () => {
  return gulp.src([paths.css, "!" + paths.minCss])
    .pipe(concat(paths.concatCssDest))
    .pipe(cssmin())
    .pipe(gulp.dest("."));
});

gulp.task("min", gulp.series(["min:js", "min:css"]));

// A 'default' task is required by Gulp v4
gulp.task("default", gulp.series(["min"]));
```

---

Nella tabella seguente fornisce una spiegazione delle attività specificata nel codice precedente:

|Nome attività|Descrizione|
|--- |--- |
|Pulisci: js|Un'attività che usa il modulo di eliminazione del nodo rimraf per rimuovere la versione del file Site. js minimizzata.|
|Pulisci: css|Un'attività che usa il modulo di eliminazione del nodo rimraf per rimuovere la versione del file CSS minimizzata.|
|Pulisci|Un'attività che chiama il `clean:js` attività, aggiungendo il `clean:css` attività.|
|min:js|Un'attività che minimizza e concatena tutti i file con estensione js nella cartella js. Il. file min.js vengono esclusi.|
|min:css|Un'attività che minimizza e concatena tutti i file con estensione CSS all'interno della cartella css. Il. file min.css vengono esclusi.|
|min|Un'attività che chiama il `min:js` attività, aggiungendo il `min:css` attività.|

## <a name="running-default-tasks"></a>Esecuzione di attività predefinita

Se è ancora stata creata una nuova app Web, creare un nuovo progetto di applicazione Web ASP.NET in Visual Studio.

1. Aprire il *package. JSON* file (aggiungere se non esiste) e aggiungere il codice seguente.

    ```json
    {
      "devDependencies": {
        "gulp": "^4.0.0",
        "gulp-concat": "2.6.1",
        "gulp-cssmin": "0.2.0",
        "gulp-uglify": "3.0.0",
        "rimraf": "2.6.1"
      }
    }
    ```

2. Aggiungere un nuovo file JavaScript al progetto e denominarla *gulpfile. js*, quindi copiare il codice seguente.

    ```javascript
    /// <binding Clean='clean' />
    "use strict";

    const gulp = require("gulp"),
          rimraf = require("rimraf"),
          concat = require("gulp-concat"),
          cssmin = require("gulp-cssmin"),
          uglify = require("gulp-uglify");

    const paths = {
      webroot: "./wwwroot/"
    };

    paths.js = paths.webroot + "js/**/*.js";
    paths.minJs = paths.webroot + "js/**/*.min.js";
    paths.css = paths.webroot + "css/**/*.css";
    paths.minCss = paths.webroot + "css/**/*.min.css";
    paths.concatJsDest = paths.webroot + "js/site.min.js";
    paths.concatCssDest = paths.webroot + "css/site.min.css";

    gulp.task("clean:js", done => rimraf(paths.concatJsDest, done));
    gulp.task("clean:css", done => rimraf(paths.concatCssDest, done));
    gulp.task("clean", gulp.series(["clean:js", "clean:css"]));

    gulp.task("min:js", () => {
      return gulp.src([paths.js, "!" + paths.minJs], { base: "." })
      .pipe(concat(paths.concatJsDest))
      .pipe(uglify())
      .pipe(gulp.dest("."));
    });

    gulp.task("min:css", () => {
      return gulp.src([paths.css, "!" + paths.minCss])
      .pipe(concat(paths.concatCssDest))
      .pipe(cssmin())
      .pipe(gulp.dest("."));
    });

    gulp.task("min", gulp.series(["min:js", "min:css"]));

    // A 'default' task is required by Gulp v4
    gulp.task("default", gulp.series(["min"]));
    ```

3. Nelle **Esplora soluzioni**, fare doppio clic su *gulpfile. js*e selezionare **Task Runner Explorer**.

    ![Aprire Task Runner Explorer da Esplora soluzioni](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)

    **Esplora esecuzione attività** Mostra l'elenco di attività Gulp. (Potrebbe essere necessario scegliere il **Aggiorna** pulsante visualizzato a sinistra del nome del progetto.)

    ![Task Runner Explorer](using-gulp/_static/03-TaskRunnerExplorer.png)

    > [!IMPORTANT]
    > Il **Task Runner Explorer** menu di scelta rapida viene visualizzata solo se *gulpfile. js* è nella directory radice del progetto.

4. Trova di sotto **attività** nelle **Task Runner Explorer**, fare doppio clic su **pulita**e selezionare **eseguire** nel menu a comparsa.

    ![Attività clean Esplora esecuzione attività](using-gulp/_static/04-TaskRunner-clean.png)

    **Esplora esecuzione attività** creerà una nuova scheda denominata **pulita** ed eseguire l'attività clean come definito nella *gulpfile. js*.

5. Fare doppio clic il **pulita** attività, quindi selezionare **associazioni** > **prima di compilare**.

    ![Task Runner Explorer BeforeBuild di associazione](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    Il **prima di compilare** associazione consente di configurare l'attività clean per l'esecuzione automatica prima di ogni compilazione del progetto.

Le associazioni impostato con **Task Runner Explorer** vengono archiviati sotto forma di un commento nella parte superiore delle *gulpfile. js* e diventano effettive solo in Visual Studio. Alternativa che non richiede Visual Studio, è possibile configurare l'esecuzione automatica di attività gulp nelle *file con estensione csproj* file. Ad esempio, inserire il codice seguente *file con estensione csproj* file:

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

A questo punto l'attività di pulizia viene eseguito quando si esegue il progetto in Visual Studio o da un prompt dei comandi usando il [dotnet eseguiti](/dotnet/core/tools/dotnet-run) comando (eseguire `npm install` prima).

## <a name="defining-and-running-a-new-task"></a>Definizione e l'esecuzione di una nuova attività

Per definire una nuova attività Gulp, modificare *gulpfile. js*.

1. Aggiungere il codice JavaScript seguente alla fine della *gulpfile. js*:

    ```javascript
    gulp.task('first', done => {
      console.log('first task! <-----');
      done(); // signal completion
    });
    ```

    Questa attività è denominata `first`, e visualizza semplicemente una stringa.

2. Salvare *gulpfile. js*.

3. Nelle **Esplora soluzioni**, fare doppio clic su *gulpfile. js*e selezionare *Task Runner Explorer*.

4. Nelle **Task Runner Explorer**, fare doppio clic su **primo**e selezionare **eseguire**.

    ![Task Runner Explorer eseguire prima attività](using-gulp/_static/06-TaskRunner-First.png)

    Viene visualizzato il testo di output. Per esempi basati su scenari comuni, vedere [Gulp recipe](#gulp-recipes).

## <a name="defining-and-running-tasks-in-a-series"></a>Definizione e l'esecuzione di attività in una serie

Quando si eseguono più attività, le attività eseguite simultaneamente per impostazione predefinita. Tuttavia, se è necessario eseguire le attività in un ordine specifico, è necessario specificare quando ogni attività viene completata, nonché come le attività che dipendono dal completamento di un'altra attività.

1. Per definire una serie di attività da eseguire in ordine, sostituire il `first` attività aggiunta in precedenza *gulpfile. js* con quanto segue:

    ```javascript
    gulp.task('series:first', done => {
      console.log('first task! <-----');
      done(); // signal completion
    });
    gulp.task('series:second', done => {
      console.log('second task! <-----');
        done(); // signal completion
    });

    gulp.task('series', gulp.series(['series:first', 'series:second']), () => { });

    // A 'default' task is required by Gulp v4
      gulp.task('default', gulp.series('series'));
    ```

    Sono ora tre attività: `series:first`, `series:second`, e `series`. Il `series:second` attività include un secondo parametro che specifica una matrice di attività deve essere eseguita e completata prima di `series:second` attività verrà eseguita. Come specificato nel codice precedente solo il `series:first` attività da completare prima il `series:second` attività verrà eseguita.

2. Salvare *gulpfile. js*.

3. Nelle **Esplora soluzioni**, fare doppio clic su *gulpfile. js* e selezionare **Task Runner Explorer** se non è già aperto.

4. Nelle **Task Runner Explorer**, fare doppio clic su **serie** e selezionare **eseguire**.

    ![Task Runner Explorer eseguire attività di serie](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a>IntelliSense

IntelliSense offre il completamento del codice, le descrizioni dei parametri e altre funzionalità per migliorare la produttività e ridurre gli errori. Le attività gulp sono scritte in JavaScript. Pertanto, IntelliSense può fornire assistenza durante lo sviluppo. Quando si lavora con JavaScript, IntelliSense Elenca di oggetti, funzioni, proprietà e i parametri che sono disponibili in base al contesto corrente. Selezionare un'opzione di codifica dall'elenco popup fornito da IntelliSense per completare il codice.

![gulp IntelliSense](using-gulp/_static/08-IntelliSense.png)

Per altre informazioni su IntelliSense, vedere [JavaScript IntelliSense](/visualstudio/ide/javascript-intellisense).

## <a name="development-staging-and-production-environments"></a>Ambienti di sviluppo, staging e produzione

Quando si Gulp viene usato per ottimizzare i file lato client per lo staging e produzione, i file elaborati vengono salvati in un percorso locale di gestione temporanea e produzione. Il *layout. cshtml* file Usa il **ambiente** per fornire due diverse versioni dei file CSS di helper tag. È una versione del file CSS per lo sviluppo e l'altra versione è ottimizzata per la gestione temporanea e produzione. In Visual Studio 2017, quando si modifica il **ASPNETCORE_ENVIRONMENT** variabile di ambiente `Production`, Visual Studio creerà l'app Web e un collegamento ai file CSS ridotta a icona. Il markup seguente mostra la **ambiente** contenente i tag di collegamento per gli helper tag di `Development` CSS file e il minimizzata `Staging, Production` file CSS.

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

## <a name="switching-between-environments"></a>Il passaggio tra gli ambienti

Per spostarsi tra la compilazione per ambienti diversi, modificare il **ASPNETCORE_ENVIRONMENT** valore della variabile di ambiente.

1. Nelle **Task Runner Explorer**, verificare che il **min** attività è stata impostata per l'esecuzione **prima di compilare**.

2. Nelle **Esplora soluzioni**, fare clic sul nome del progetto e selezionare **proprietà**.

    Viene visualizzata la finestra delle proprietà per l'app Web.

3. Fare clic sulla scheda **Debug**.

4. Impostare il valore della **: l'ambiente di Hosting** variabile di ambiente `Production`.

5. Premere **F5** per eseguire l'applicazione in un browser.

6. Nella finestra del browser, la pagina e scegliere **Visualizza origine** per visualizzare il codice HTML della pagina.

    Si noti che i collegamenti di foglio di stile puntano ai file CSS minimizzati.

7. Chiudere il browser per arrestare l'app Web.

8. In Visual Studio, tornare alla finestra delle proprietà per l'app Web e modificare il **: l'ambiente di Hosting** variabile di ambiente verso `Development`.

9. Premere **F5** per eseguire nuovamente l'applicazione in un browser.

10. Nella finestra del browser, la pagina e scegliere **Visualizza origine** per visualizzare il codice HTML della pagina.

    Si noti che i collegamenti di foglio di stile puntino alle versioni non minimizzate dei file CSS.

Per altre informazioni correlate agli ambienti in ASP.NET Core, vedere [usare più ambienti](../fundamentals/environments.md).

## <a name="task-and-module-details"></a>Dettagli attività e il modulo

Un'attività Gulp è registrata con un nome di funzione. È possibile specificare le dipendenze se necessario eseguire altre attività prima dell'attività corrente. Funzioni aggiuntive consentono di eseguire e controllare le attività Gulp, nonché di impostare l'origine (*src*) e di destinazione (*dest*) dei file da modificare. Di seguito sono le funzioni API Gulp principali:

|Gulp (funzione)|Sintassi|Descrizione|
|---   |--- |--- |
|attività  |`gulp.task(name[, deps], fn) { }`|Il `task` funzione crea un'attività. Il `name` parametro definisce il nome dell'attività. Il `deps` parametro contiene una matrice di attività da completare prima di questa attività viene eseguita. Il `fn` parametro rappresenta una funzione di callback che consente di eseguire le operazioni dell'attività.|
|Espressioni di controllo |`gulp.watch(glob [, opts], tasks) { }`|Il `watch` funzione consente di monitorare le attività di file e viene eseguito quando si verifica una modifica del file. Il `glob` parametro è un `string` o `array` che determina quali file da controllare. Il `opts` parametro fornisce la visione di opzioni aggiuntive del file.|
|src   |`gulp.src(globs[, options]) { }`|Il `src` funzione fornisce i file che corrispondono al valore/i glob. Il `glob` parametro è un `string` o `array` che determina quali file da leggere. Il `options` parametro fornisce opzioni aggiuntive del file.|
|dest  |`gulp.dest(path[, options]) { }`|Il `dest` funzione definisce una posizione in cui è possono scrivere i file. Il `path` parametro è una stringa o una funzione che determina la cartella di destinazione. Il `options` parametro è un oggetto che specifica le opzioni cartella di output.|

Per altre informazioni di riferimento API di Gulp, vedere [Gulp documentazione API](https://gulpjs.org/API.html).

## <a name="gulp-recipes"></a>Recipe di gulp

La community di Gulp offre Gulp [recipe](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md). Queste soluzioni sono costituiti da attività Gulp per scenari comuni.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Documentazione di gulp](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [Creazione di bundle e minimizzazione in ASP.NET Core](bundling-and-minification.md)
* [Usare Grunt in ASP.NET Core](using-grunt.md)
