---
title: Usare Gulp in ASP.NET Core
author: rick-anderson
description: Informazioni su come usare Gulp in ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/04/2018
uid: client-side/using-gulp
ms.openlocfilehash: e280eabecbd427f3e1418b3d7a60e0ea3df46a5a
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450606"
---
# <a name="use-gulp-in-aspnet-core"></a><span data-ttu-id="1ba59-103">Usare Gulp in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1ba59-103">Use Gulp in ASP.NET Core</span></span>

<span data-ttu-id="1ba59-104">Dal [Scott Addie](https://scottaddie.com), [Shayne Boyer](https://twitter.com/spboyer), e [pino David](https://twitter.com/davidpine7)</span><span class="sxs-lookup"><span data-stu-id="1ba59-104">By [Scott Addie](https://scottaddie.com), [Shayne Boyer](https://twitter.com/spboyer), and [David Pine](https://twitter.com/davidpine7)</span></span>

<span data-ttu-id="1ba59-105">In un'app web moderne tipico, potrebbe essere il processo di compilazione:</span><span class="sxs-lookup"><span data-stu-id="1ba59-105">In a typical modern web app, the build process might:</span></span>

* <span data-ttu-id="1ba59-106">Aggregare e minimizzare file CSS e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1ba59-106">Bundle and minify JavaScript and CSS files.</span></span>
* <span data-ttu-id="1ba59-107">Eseguire gli strumenti per chiamare le operazioni di creazione di bundle e minimizzazione prima di ogni compilazione.</span><span class="sxs-lookup"><span data-stu-id="1ba59-107">Run tools to call the bundling and minification tasks before each build.</span></span>
* <span data-ttu-id="1ba59-108">Compilare meno o il SASS file CSS.</span><span class="sxs-lookup"><span data-stu-id="1ba59-108">Compile LESS or SASS files to CSS.</span></span>
* <span data-ttu-id="1ba59-109">Compilare i file TypeScript o CoffeeScript a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="1ba59-109">Compile CoffeeScript or TypeScript files to JavaScript.</span></span>

<span data-ttu-id="1ba59-110">Oggetto *strumento di esecuzione attività* è uno strumento che consente di automatizzare le attività di sviluppo di routine e altro ancora.</span><span class="sxs-lookup"><span data-stu-id="1ba59-110">A *task runner* is a tool which automates these routine development tasks and more.</span></span> <span data-ttu-id="1ba59-111">Visual Studio fornisce supporto incorporato per gli strumenti di esecuzione di due attività basate su JavaScript più diffusi: [Gulp](https://gulpjs.com/) e [Grunt](using-grunt.md).</span><span class="sxs-lookup"><span data-stu-id="1ba59-111">Visual Studio provides built-in support for two popular JavaScript-based task runners: [Gulp](https://gulpjs.com/) and [Grunt](using-grunt.md).</span></span>

## <a name="gulp"></a><span data-ttu-id="1ba59-112">Gulp</span><span class="sxs-lookup"><span data-stu-id="1ba59-112">Gulp</span></span>

<span data-ttu-id="1ba59-113">Gulp è un basate su JavaScript streaming compilazione toolkit per il codice lato client.</span><span class="sxs-lookup"><span data-stu-id="1ba59-113">Gulp is a JavaScript-based streaming build toolkit for client-side code.</span></span> <span data-ttu-id="1ba59-114">Viene comunemente utilizzato per trasmettere file lato client tramite una serie di processi quando viene generato un evento specifico in un ambiente di compilazione.</span><span class="sxs-lookup"><span data-stu-id="1ba59-114">It's commonly used to stream client-side files through a series of processes when a specific event is triggered in a build environment.</span></span> <span data-ttu-id="1ba59-115">Ad esempio, Gulp può essere utilizzato per automatizzare [creazione di bundle e minimizzazione](bundling-and-minification.md) o la pulizia di un ambiente di sviluppo prima di una nuova compilazione.</span><span class="sxs-lookup"><span data-stu-id="1ba59-115">For instance, Gulp can be used to automate [bundling and minification](bundling-and-minification.md) or the cleaning of a development environment before a new build.</span></span>

<span data-ttu-id="1ba59-116">È definito un set di attività Gulp *gulpfile. js*.</span><span class="sxs-lookup"><span data-stu-id="1ba59-116">A set of Gulp tasks is defined in *gulpfile.js*.</span></span> <span data-ttu-id="1ba59-117">Il codice JavaScript seguente include i moduli di Gulp e specifica i percorsi di file a cui fare riferimento all'interno delle attività future:</span><span class="sxs-lookup"><span data-stu-id="1ba59-117">The following JavaScript includes Gulp modules and specifies file paths to be referenced within the forthcoming tasks:</span></span>

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

<span data-ttu-id="1ba59-118">Il codice sopra riportato specifica che i moduli del nodo sono necessari.</span><span class="sxs-lookup"><span data-stu-id="1ba59-118">The above code specifies which Node modules are required.</span></span> <span data-ttu-id="1ba59-119">Il `require` ogni modulo importazioni di funzioni in modo che le attività dipendenti possono usare le funzionalità incluse.</span><span class="sxs-lookup"><span data-stu-id="1ba59-119">The `require` function imports each module so that the dependent tasks can utilize their features.</span></span> <span data-ttu-id="1ba59-120">Ognuno dei moduli importati viene assegnato a una variabile.</span><span class="sxs-lookup"><span data-stu-id="1ba59-120">Each of the imported modules is assigned to a variable.</span></span> <span data-ttu-id="1ba59-121">I moduli possono trovarsi in base al nome o percorso.</span><span class="sxs-lookup"><span data-stu-id="1ba59-121">The modules can be located either by name or path.</span></span> <span data-ttu-id="1ba59-122">In questo esempio, i moduli denominati `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, e `gulp-uglify` vengono recuperati in base al nome.</span><span class="sxs-lookup"><span data-stu-id="1ba59-122">In this example, the modules named `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, and `gulp-uglify` are retrieved by name.</span></span> <span data-ttu-id="1ba59-123">Inoltre, una serie di tracciati vengono creati in modo che i percorsi dei file CSS e JavaScript possono essere riutilizzati e fa riferimento all'interno delle attività.</span><span class="sxs-lookup"><span data-stu-id="1ba59-123">Additionally, a series of paths are created so that the locations of CSS and JavaScript files can be reused and referenced within the tasks.</span></span> <span data-ttu-id="1ba59-124">Nella tabella seguente fornisce le descrizioni dei moduli inclusi in *gulpfile. js*.</span><span class="sxs-lookup"><span data-stu-id="1ba59-124">The following table provides descriptions of the modules included in *gulpfile.js*.</span></span>

| <span data-ttu-id="1ba59-125">Nome modulo</span><span class="sxs-lookup"><span data-stu-id="1ba59-125">Module Name</span></span> | <span data-ttu-id="1ba59-126">Descrizione</span><span class="sxs-lookup"><span data-stu-id="1ba59-126">Description</span></span> |
| ----------- | ----------- |
| <span data-ttu-id="1ba59-127">Gulp</span><span class="sxs-lookup"><span data-stu-id="1ba59-127">gulp</span></span>        | <span data-ttu-id="1ba59-128">Il sistema di compilazione streaming Gulp.</span><span class="sxs-lookup"><span data-stu-id="1ba59-128">The Gulp streaming build system.</span></span> <span data-ttu-id="1ba59-129">Per altre informazioni, vedere [gulp](https://www.npmjs.com/package/gulp).</span><span class="sxs-lookup"><span data-stu-id="1ba59-129">For more information, see [gulp](https://www.npmjs.com/package/gulp).</span></span> |
| <span data-ttu-id="1ba59-130">rimraf</span><span class="sxs-lookup"><span data-stu-id="1ba59-130">rimraf</span></span>      | <span data-ttu-id="1ba59-131">Un modulo di eliminazione del nodo.</span><span class="sxs-lookup"><span data-stu-id="1ba59-131">A Node deletion module.</span></span> <span data-ttu-id="1ba59-132">Per altre informazioni, vedere [rimraf](https://www.npmjs.com/package/rimraf).</span><span class="sxs-lookup"><span data-stu-id="1ba59-132">For more information, see [rimraf](https://www.npmjs.com/package/rimraf).</span></span> |
| <span data-ttu-id="1ba59-133">gulp concat</span><span class="sxs-lookup"><span data-stu-id="1ba59-133">gulp-concat</span></span> | <span data-ttu-id="1ba59-134">Un modulo che concatena i file in base al carattere di nuova riga del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="1ba59-134">A module that concatenates files based on the operating system's newline character.</span></span> <span data-ttu-id="1ba59-135">Per altre informazioni, vedere [gulp concat](https://www.npmjs.com/package/gulp-concat).</span><span class="sxs-lookup"><span data-stu-id="1ba59-135">For more information, see [gulp-concat](https://www.npmjs.com/package/gulp-concat).</span></span> |
| <span data-ttu-id="1ba59-136">gulp cssmin</span><span class="sxs-lookup"><span data-stu-id="1ba59-136">gulp-cssmin</span></span> | <span data-ttu-id="1ba59-137">Un modulo che minimizza file CSS.</span><span class="sxs-lookup"><span data-stu-id="1ba59-137">A module that minifies CSS files.</span></span> <span data-ttu-id="1ba59-138">Per altre informazioni, vedere [gulp cssmin](https://www.npmjs.com/package/gulp-cssmin).</span><span class="sxs-lookup"><span data-stu-id="1ba59-138">For more information, see [gulp-cssmin](https://www.npmjs.com/package/gulp-cssmin).</span></span> |
| <span data-ttu-id="1ba59-139">uglify gulp</span><span class="sxs-lookup"><span data-stu-id="1ba59-139">gulp-uglify</span></span> | <span data-ttu-id="1ba59-140">Un modulo che minimizza *js* file.</span><span class="sxs-lookup"><span data-stu-id="1ba59-140">A module that minifies *.js* files.</span></span> <span data-ttu-id="1ba59-141">Per altre informazioni, vedere [uglify gulp](https://www.npmjs.com/package/gulp-uglify).</span><span class="sxs-lookup"><span data-stu-id="1ba59-141">For more information, see [gulp-uglify](https://www.npmjs.com/package/gulp-uglify).</span></span> |

<span data-ttu-id="1ba59-142">Dopo aver importati i moduli richiesti, è possibile specificare le attività.</span><span class="sxs-lookup"><span data-stu-id="1ba59-142">Once the requisite modules are imported, the tasks can be specified.</span></span> <span data-ttu-id="1ba59-143">Di seguito sono presenti sei attività registrate, rappresentata nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="1ba59-143">Here there are six tasks registered, represented by the following code:</span></span>

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

<span data-ttu-id="1ba59-144">Nella tabella seguente fornisce una spiegazione delle attività specificata nel codice precedente:</span><span class="sxs-lookup"><span data-stu-id="1ba59-144">The following table provides an explanation of the tasks specified in the code above:</span></span>

|<span data-ttu-id="1ba59-145">Nome attività</span><span class="sxs-lookup"><span data-stu-id="1ba59-145">Task Name</span></span>|<span data-ttu-id="1ba59-146">Descrizione</span><span class="sxs-lookup"><span data-stu-id="1ba59-146">Description</span></span>|
|--- |--- |
|<span data-ttu-id="1ba59-147">Pulisci: js</span><span class="sxs-lookup"><span data-stu-id="1ba59-147">clean:js</span></span>|<span data-ttu-id="1ba59-148">Un'attività che usa il modulo di eliminazione del nodo rimraf per rimuovere la versione del file Site. js minimizzata.</span><span class="sxs-lookup"><span data-stu-id="1ba59-148">A task that uses the rimraf Node deletion module to remove the minified version of the site.js file.</span></span>|
|<span data-ttu-id="1ba59-149">Pulisci: css</span><span class="sxs-lookup"><span data-stu-id="1ba59-149">clean:css</span></span>|<span data-ttu-id="1ba59-150">Un'attività che usa il modulo di eliminazione del nodo rimraf per rimuovere la versione del file CSS minimizzata.</span><span class="sxs-lookup"><span data-stu-id="1ba59-150">A task that uses the rimraf Node deletion module to remove the minified version of the site.css file.</span></span>|
|<span data-ttu-id="1ba59-151">Pulisci</span><span class="sxs-lookup"><span data-stu-id="1ba59-151">clean</span></span>|<span data-ttu-id="1ba59-152">Un'attività che chiama il `clean:js` attività, aggiungendo il `clean:css` attività.</span><span class="sxs-lookup"><span data-stu-id="1ba59-152">A task that calls the `clean:js` task, followed by the `clean:css` task.</span></span>|
|<span data-ttu-id="1ba59-153">min:js</span><span class="sxs-lookup"><span data-stu-id="1ba59-153">min:js</span></span>|<span data-ttu-id="1ba59-154">Un'attività che minimizza e concatena tutti i file con estensione js nella cartella js.</span><span class="sxs-lookup"><span data-stu-id="1ba59-154">A task that minifies and concatenates all .js files within the js folder.</span></span> <span data-ttu-id="1ba59-155">Il. file min.js vengono esclusi.</span><span class="sxs-lookup"><span data-stu-id="1ba59-155">The .min.js files are excluded.</span></span>|
|<span data-ttu-id="1ba59-156">min:css</span><span class="sxs-lookup"><span data-stu-id="1ba59-156">min:css</span></span>|<span data-ttu-id="1ba59-157">Un'attività che minimizza e concatena tutti i file con estensione CSS all'interno della cartella css.</span><span class="sxs-lookup"><span data-stu-id="1ba59-157">A task that minifies and concatenates all .css files within the css folder.</span></span> <span data-ttu-id="1ba59-158">Il. file min.css vengono esclusi.</span><span class="sxs-lookup"><span data-stu-id="1ba59-158">The .min.css files are excluded.</span></span>|
|<span data-ttu-id="1ba59-159">min</span><span class="sxs-lookup"><span data-stu-id="1ba59-159">min</span></span>|<span data-ttu-id="1ba59-160">Un'attività che chiama il `min:js` attività, aggiungendo il `min:css` attività.</span><span class="sxs-lookup"><span data-stu-id="1ba59-160">A task that calls the `min:js` task, followed by the `min:css` task.</span></span>|

## <a name="running-default-tasks"></a><span data-ttu-id="1ba59-161">Esecuzione di attività predefinita</span><span class="sxs-lookup"><span data-stu-id="1ba59-161">Running default tasks</span></span>

<span data-ttu-id="1ba59-162">Se è ancora stata creata una nuova app Web, creare un nuovo progetto di applicazione Web ASP.NET in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1ba59-162">If you haven't already created a new Web app, create a new ASP.NET Web Application project in Visual Studio.</span></span>

1.  <span data-ttu-id="1ba59-163">Aprire il *package. JSON* file (aggiungere se non esiste) e aggiungere il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="1ba59-163">Open the *package.json* file (add if not there) and add the following.</span></span>

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

2.  <span data-ttu-id="1ba59-164">Aggiungere un nuovo file JavaScript al progetto e denominarla *gulpfile. js*, quindi copiare il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="1ba59-164">Add a new JavaScript file to your project and name it *gulpfile.js*, then copy the following code.</span></span>

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

3.  <span data-ttu-id="1ba59-165">Nelle **Esplora soluzioni**, fare doppio clic su *gulpfile. js*e selezionare **Task Runner Explorer**.</span><span class="sxs-lookup"><span data-stu-id="1ba59-165">In **Solution Explorer**, right-click *gulpfile.js*, and select **Task Runner Explorer**.</span></span>
    
    ![Aprire Task Runner Explorer da Esplora soluzioni](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    <span data-ttu-id="1ba59-167">**Esplora esecuzione attività** Mostra l'elenco di attività Gulp.</span><span class="sxs-lookup"><span data-stu-id="1ba59-167">**Task Runner Explorer** shows the list of Gulp tasks.</span></span> <span data-ttu-id="1ba59-168">(Potrebbe essere necessario scegliere il **Aggiorna** pulsante visualizzato a sinistra del nome del progetto.)</span><span class="sxs-lookup"><span data-stu-id="1ba59-168">(You might have to click the **Refresh** button that appears to the left of the project name.)</span></span>
    
    ![Task Runner Explorer](using-gulp/_static/03-TaskRunnerExplorer.png)
    
    > [!IMPORTANT]
    > <span data-ttu-id="1ba59-170">Il **Task Runner Explorer** menu di scelta rapida viene visualizzata solo se *gulpfile. js* è nella directory radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="1ba59-170">The **Task Runner Explorer** context menu item appears only if *gulpfile.js* is in the root project directory.</span></span>

4.  <span data-ttu-id="1ba59-171">Trova di sotto **attività** nelle **Task Runner Explorer**, fare doppio clic su **pulita**e selezionare **eseguire** nel menu a comparsa.</span><span class="sxs-lookup"><span data-stu-id="1ba59-171">Underneath **Tasks** in **Task Runner Explorer**, right-click **clean**, and select **Run** from the pop-up menu.</span></span>

    ![Attività clean Esplora esecuzione attività](using-gulp/_static/04-TaskRunner-clean.png)

    <span data-ttu-id="1ba59-173">**Esplora esecuzione attività** creerà una nuova scheda denominata **pulita** ed eseguire l'attività clean come definito nella *gulpfile. js*.</span><span class="sxs-lookup"><span data-stu-id="1ba59-173">**Task Runner Explorer** will create a new tab named **clean** and execute the clean task as it's defined in *gulpfile.js*.</span></span>

5.  <span data-ttu-id="1ba59-174">Fare doppio clic il **pulita** attività, quindi selezionare **associazioni** > **prima di compilare**.</span><span class="sxs-lookup"><span data-stu-id="1ba59-174">Right-click the **clean** task, then select **Bindings** > **Before Build**.</span></span>

    ![Task Runner Explorer BeforeBuild di associazione](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    <span data-ttu-id="1ba59-176">Il **prima di compilare** associazione consente di configurare l'attività clean per l'esecuzione automatica prima di ogni compilazione del progetto.</span><span class="sxs-lookup"><span data-stu-id="1ba59-176">The **Before Build** binding configures the clean task to run automatically before each build of the project.</span></span>

<span data-ttu-id="1ba59-177">Le associazioni impostato con **Task Runner Explorer** vengono archiviati sotto forma di un commento nella parte superiore delle *gulpfile. js* e diventano effettive solo in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1ba59-177">The bindings you set up with **Task Runner Explorer** are stored in the form of a comment at the top of your *gulpfile.js* and are effective only in Visual Studio.</span></span> <span data-ttu-id="1ba59-178">Alternativa che non richiede Visual Studio, è possibile configurare l'esecuzione automatica di attività gulp nelle *file con estensione csproj* file.</span><span class="sxs-lookup"><span data-stu-id="1ba59-178">An alternative that doesn't require Visual Studio is to configure automatic execution of gulp tasks in your *.csproj* file.</span></span> <span data-ttu-id="1ba59-179">Ad esempio, inserire il codice seguente *file con estensione csproj* file:</span><span class="sxs-lookup"><span data-stu-id="1ba59-179">For example, put this in your *.csproj* file:</span></span>

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

<span data-ttu-id="1ba59-180">A questo punto l'attività di pulizia viene eseguito quando si esegue il progetto in Visual Studio o da un prompt dei comandi usando il [dotnet eseguiti](/dotnet/core/tools/dotnet-run) comando (eseguire `npm install` prima).</span><span class="sxs-lookup"><span data-stu-id="1ba59-180">Now the clean task is executed when you run the project in Visual Studio or from a command prompt using the [dotnet run](/dotnet/core/tools/dotnet-run) command (run `npm install` first).</span></span>

## <a name="defining-and-running-a-new-task"></a><span data-ttu-id="1ba59-181">Definizione e l'esecuzione di una nuova attività</span><span class="sxs-lookup"><span data-stu-id="1ba59-181">Defining and running a new task</span></span>

<span data-ttu-id="1ba59-182">Per definire una nuova attività Gulp, modificare *gulpfile. js*.</span><span class="sxs-lookup"><span data-stu-id="1ba59-182">To define a new Gulp task, modify *gulpfile.js*.</span></span>

1.  <span data-ttu-id="1ba59-183">Aggiungere il codice JavaScript seguente alla fine della *gulpfile. js*:</span><span class="sxs-lookup"><span data-stu-id="1ba59-183">Add the following JavaScript to the end of *gulpfile.js*:</span></span>

    ```javascript
    gulp.task('first', done => {
      console.log('first task! <-----');
      done(); // signal completion
    });
    ```

    <span data-ttu-id="1ba59-184">Questa attività è denominata `first`, e visualizza semplicemente una stringa.</span><span class="sxs-lookup"><span data-stu-id="1ba59-184">This task is named `first`, and it simply displays a string.</span></span>

2.  <span data-ttu-id="1ba59-185">Salvare *gulpfile. js*.</span><span class="sxs-lookup"><span data-stu-id="1ba59-185">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="1ba59-186">Nelle **Esplora soluzioni**, fare doppio clic su *gulpfile. js*e selezionare *Task Runner Explorer*.</span><span class="sxs-lookup"><span data-stu-id="1ba59-186">In **Solution Explorer**, right-click *gulpfile.js*, and select *Task Runner Explorer*.</span></span>

4.  <span data-ttu-id="1ba59-187">Nelle **Task Runner Explorer**, fare doppio clic su **primo**e selezionare **eseguire**.</span><span class="sxs-lookup"><span data-stu-id="1ba59-187">In **Task Runner Explorer**, right-click **first**, and select **Run**.</span></span>

    ![Task Runner Explorer eseguire prima attività](using-gulp/_static/06-TaskRunner-First.png)

    <span data-ttu-id="1ba59-189">Viene visualizzato il testo di output.</span><span class="sxs-lookup"><span data-stu-id="1ba59-189">The output text is displayed.</span></span> <span data-ttu-id="1ba59-190">Per esempi basati su scenari comuni, vedere [Gulp recipe](#gulp-recipes).</span><span class="sxs-lookup"><span data-stu-id="1ba59-190">To see examples based on common scenarios, see [Gulp Recipes](#gulp-recipes).</span></span>

## <a name="defining-and-running-tasks-in-a-series"></a><span data-ttu-id="1ba59-191">Definizione e l'esecuzione di attività in una serie</span><span class="sxs-lookup"><span data-stu-id="1ba59-191">Defining and running tasks in a series</span></span>

<span data-ttu-id="1ba59-192">Quando si eseguono più attività, le attività eseguite simultaneamente per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="1ba59-192">When you run multiple tasks, the tasks run concurrently by default.</span></span> <span data-ttu-id="1ba59-193">Tuttavia, se è necessario eseguire le attività in un ordine specifico, è necessario specificare quando ogni attività viene completata, nonché come le attività che dipendono dal completamento di un'altra attività.</span><span class="sxs-lookup"><span data-stu-id="1ba59-193">However, if you need to run tasks in a specific order, you must specify when each task is complete, as well as which tasks depend on the completion of another task.</span></span>

1.  <span data-ttu-id="1ba59-194">Per definire una serie di attività da eseguire in ordine, sostituire il `first` attività aggiunta in precedenza *gulpfile. js* con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="1ba59-194">To define a series of tasks to run in order, replace the `first` task that you added above in *gulpfile.js* with the following:</span></span>

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
 
    <span data-ttu-id="1ba59-195">Sono ora tre attività: `series:first`, `series:second`, e `series`.</span><span class="sxs-lookup"><span data-stu-id="1ba59-195">You now have three tasks: `series:first`, `series:second`, and `series`.</span></span> <span data-ttu-id="1ba59-196">Il `series:second` attività include un secondo parametro che specifica una matrice di attività deve essere eseguita e completata prima di `series:second` attività verrà eseguita.</span><span class="sxs-lookup"><span data-stu-id="1ba59-196">The `series:second` task includes a second parameter which specifies an array of tasks to be run and completed before the `series:second` task will run.</span></span> <span data-ttu-id="1ba59-197">Come specificato nel codice precedente solo il `series:first` attività da completare prima il `series:second` attività verrà eseguita.</span><span class="sxs-lookup"><span data-stu-id="1ba59-197">As specified in the code above, only the `series:first` task must be completed before the `series:second` task will run.</span></span>

2.  <span data-ttu-id="1ba59-198">Salvare *gulpfile. js*.</span><span class="sxs-lookup"><span data-stu-id="1ba59-198">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="1ba59-199">Nelle **Esplora soluzioni**, fare doppio clic su *gulpfile. js* e selezionare **Task Runner Explorer** se non è già aperto.</span><span class="sxs-lookup"><span data-stu-id="1ba59-199">In **Solution Explorer**, right-click *gulpfile.js* and select **Task Runner Explorer** if it isn't already open.</span></span>

4.  <span data-ttu-id="1ba59-200">Nelle **Task Runner Explorer**, fare doppio clic su **serie** e selezionare **eseguire**.</span><span class="sxs-lookup"><span data-stu-id="1ba59-200">In **Task Runner Explorer**, right-click **series** and select **Run**.</span></span>

    ![Task Runner Explorer eseguire attività di serie](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a><span data-ttu-id="1ba59-202">IntelliSense</span><span class="sxs-lookup"><span data-stu-id="1ba59-202">IntelliSense</span></span>

<span data-ttu-id="1ba59-203">IntelliSense offre il completamento del codice, le descrizioni dei parametri e altre funzionalità per migliorare la produttività e ridurre gli errori.</span><span class="sxs-lookup"><span data-stu-id="1ba59-203">IntelliSense provides code completion, parameter descriptions, and other features to boost productivity and to decrease errors.</span></span> <span data-ttu-id="1ba59-204">Le attività gulp sono scritte in JavaScript. Pertanto, IntelliSense può fornire assistenza durante lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="1ba59-204">Gulp tasks are written in JavaScript; therefore, IntelliSense can provide assistance while developing.</span></span> <span data-ttu-id="1ba59-205">Quando si lavora con JavaScript, IntelliSense Elenca di oggetti, funzioni, proprietà e i parametri che sono disponibili in base al contesto corrente.</span><span class="sxs-lookup"><span data-stu-id="1ba59-205">As you work with JavaScript, IntelliSense lists the objects, functions, properties, and parameters that are available based on your current context.</span></span> <span data-ttu-id="1ba59-206">Selezionare un'opzione di codifica dall'elenco popup fornito da IntelliSense per completare il codice.</span><span class="sxs-lookup"><span data-stu-id="1ba59-206">Select a coding option from the pop-up list provided by IntelliSense to complete the code.</span></span>

![gulp IntelliSense](using-gulp/_static/08-IntelliSense.png)

<span data-ttu-id="1ba59-208">Per altre informazioni su IntelliSense, vedere [JavaScript IntelliSense](/visualstudio/ide/javascript-intellisense).</span><span class="sxs-lookup"><span data-stu-id="1ba59-208">For more information about IntelliSense, see [JavaScript IntelliSense](/visualstudio/ide/javascript-intellisense).</span></span>

## <a name="development-staging-and-production-environments"></a><span data-ttu-id="1ba59-209">Ambienti di sviluppo, staging e produzione</span><span class="sxs-lookup"><span data-stu-id="1ba59-209">Development, staging, and production environments</span></span>

<span data-ttu-id="1ba59-210">Quando si Gulp viene usato per ottimizzare i file lato client per lo staging e produzione, i file elaborati vengono salvati in un percorso locale di gestione temporanea e produzione.</span><span class="sxs-lookup"><span data-stu-id="1ba59-210">When Gulp is used to optimize client-side files for staging and production, the processed files are saved to a local staging and production location.</span></span> <span data-ttu-id="1ba59-211">Il *layout. cshtml* file Usa il **ambiente** per fornire due diverse versioni dei file CSS di helper tag.</span><span class="sxs-lookup"><span data-stu-id="1ba59-211">The *_Layout.cshtml* file uses the **environment** tag helper to provide two different versions of CSS files.</span></span> <span data-ttu-id="1ba59-212">È una versione del file CSS per lo sviluppo e l'altra versione è ottimizzata per la gestione temporanea e produzione.</span><span class="sxs-lookup"><span data-stu-id="1ba59-212">One version of CSS files is for development and the other version is optimized for both staging and production.</span></span> <span data-ttu-id="1ba59-213">In Visual Studio 2017, quando si modifica il **ASPNETCORE_ENVIRONMENT** variabile di ambiente `Production`, Visual Studio creerà l'app Web e un collegamento ai file CSS ridotta a icona.</span><span class="sxs-lookup"><span data-stu-id="1ba59-213">In Visual Studio 2017, when you change the **ASPNETCORE_ENVIRONMENT** environment variable to `Production`, Visual Studio will build the Web app and link to the minimized CSS files.</span></span> <span data-ttu-id="1ba59-214">Il markup seguente mostra la **ambiente** contenente i tag di collegamento per gli helper tag di `Development` CSS file e il minimizzata `Staging, Production` file CSS.</span><span class="sxs-lookup"><span data-stu-id="1ba59-214">The following markup shows the **environment** tag helpers containing link tags to the `Development` CSS files and the minified `Staging, Production` CSS files.</span></span>

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

## <a name="switching-between-environments"></a><span data-ttu-id="1ba59-215">Il passaggio tra gli ambienti</span><span class="sxs-lookup"><span data-stu-id="1ba59-215">Switching between environments</span></span>

<span data-ttu-id="1ba59-216">Per spostarsi tra la compilazione per ambienti diversi, modificare il **ASPNETCORE_ENVIRONMENT** valore della variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="1ba59-216">To switch between compiling for different environments, modify the **ASPNETCORE_ENVIRONMENT** environment variable's value.</span></span>

1.  <span data-ttu-id="1ba59-217">Nelle **Task Runner Explorer**, verificare che il **min** attività è stata impostata per l'esecuzione **prima di compilare**.</span><span class="sxs-lookup"><span data-stu-id="1ba59-217">In **Task Runner Explorer**, verify that the **min** task has been set to run **Before Build**.</span></span>

2.  <span data-ttu-id="1ba59-218">Nelle **Esplora soluzioni**, fare clic sul nome del progetto e selezionare **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="1ba59-218">In **Solution Explorer**, right-click the project name and select **Properties**.</span></span>

    <span data-ttu-id="1ba59-219">Viene visualizzata la finestra delle proprietà per l'app Web.</span><span class="sxs-lookup"><span data-stu-id="1ba59-219">The property sheet for the Web app is displayed.</span></span>

3.  <span data-ttu-id="1ba59-220">Fare clic sulla scheda **Debug**.</span><span class="sxs-lookup"><span data-stu-id="1ba59-220">Click the **Debug** tab.</span></span>

4.  <span data-ttu-id="1ba59-221">Impostare il valore della **: l'ambiente di Hosting** variabile di ambiente `Production`.</span><span class="sxs-lookup"><span data-stu-id="1ba59-221">Set the value of the **Hosting:Environment** environment variable to `Production`.</span></span>

5.  <span data-ttu-id="1ba59-222">Premere **F5** per eseguire l'applicazione in un browser.</span><span class="sxs-lookup"><span data-stu-id="1ba59-222">Press **F5** to run the application in a browser.</span></span>

6.  <span data-ttu-id="1ba59-223">Nella finestra del browser, la pagina e scegliere **Visualizza origine** per visualizzare il codice HTML della pagina.</span><span class="sxs-lookup"><span data-stu-id="1ba59-223">In the browser window, right-click the page and select **View Source** to view the HTML for the page.</span></span>

    <span data-ttu-id="1ba59-224">Si noti che i collegamenti di foglio di stile puntano ai file CSS minimizzati.</span><span class="sxs-lookup"><span data-stu-id="1ba59-224">Notice that the stylesheet links point to the minified CSS files.</span></span>

7.  <span data-ttu-id="1ba59-225">Chiudere il browser per arrestare l'app Web.</span><span class="sxs-lookup"><span data-stu-id="1ba59-225">Close the browser to stop the Web app.</span></span>

8.  <span data-ttu-id="1ba59-226">In Visual Studio, tornare alla finestra delle proprietà per l'app Web e modificare il **: l'ambiente di Hosting** variabile di ambiente verso `Development`.</span><span class="sxs-lookup"><span data-stu-id="1ba59-226">In Visual Studio, return to the property sheet for the Web app and change the **Hosting:Environment** environment variable back to `Development`.</span></span>

9.  <span data-ttu-id="1ba59-227">Premere **F5** per eseguire nuovamente l'applicazione in un browser.</span><span class="sxs-lookup"><span data-stu-id="1ba59-227">Press **F5** to run the application in a browser again.</span></span>

10. <span data-ttu-id="1ba59-228">Nella finestra del browser, la pagina e scegliere **Visualizza origine** per visualizzare il codice HTML della pagina.</span><span class="sxs-lookup"><span data-stu-id="1ba59-228">In the browser window, right-click the page and select **View Source** to see the HTML for the page.</span></span>

    <span data-ttu-id="1ba59-229">Si noti che i collegamenti di foglio di stile puntino alle versioni non minimizzate dei file CSS.</span><span class="sxs-lookup"><span data-stu-id="1ba59-229">Notice that the stylesheet links point to the unminified versions of the CSS files.</span></span>

<span data-ttu-id="1ba59-230">Per altre informazioni correlate agli ambienti in ASP.NET Core, vedere [usare più ambienti](../fundamentals/environments.md).</span><span class="sxs-lookup"><span data-stu-id="1ba59-230">For more information related to environments in ASP.NET Core, see [Use multiple environments](../fundamentals/environments.md).</span></span>

## <a name="task-and-module-details"></a><span data-ttu-id="1ba59-231">Dettagli attività e il modulo</span><span class="sxs-lookup"><span data-stu-id="1ba59-231">Task and module details</span></span>

<span data-ttu-id="1ba59-232">Un'attività Gulp è registrata con un nome di funzione.</span><span class="sxs-lookup"><span data-stu-id="1ba59-232">A Gulp task is registered with a function name.</span></span> <span data-ttu-id="1ba59-233">È possibile specificare le dipendenze se necessario eseguire altre attività prima dell'attività corrente.</span><span class="sxs-lookup"><span data-stu-id="1ba59-233">You can specify dependencies if other tasks must run before the current task.</span></span> <span data-ttu-id="1ba59-234">Funzioni aggiuntive consentono di eseguire e controllare le attività Gulp, nonché di impostare l'origine (*src*) e di destinazione (*dest*) dei file da modificare.</span><span class="sxs-lookup"><span data-stu-id="1ba59-234">Additional functions allow you to run and watch the Gulp tasks, as well as set the source (*src*) and destination (*dest*) of the files being modified.</span></span> <span data-ttu-id="1ba59-235">Di seguito sono le funzioni API Gulp principali:</span><span class="sxs-lookup"><span data-stu-id="1ba59-235">The following are the primary Gulp API functions:</span></span>

|<span data-ttu-id="1ba59-236">Gulp (funzione)</span><span class="sxs-lookup"><span data-stu-id="1ba59-236">Gulp Function</span></span>|<span data-ttu-id="1ba59-237">Sintassi</span><span class="sxs-lookup"><span data-stu-id="1ba59-237">Syntax</span></span>|<span data-ttu-id="1ba59-238">Descrizione</span><span class="sxs-lookup"><span data-stu-id="1ba59-238">Description</span></span>|
|---   |--- |--- |
|<span data-ttu-id="1ba59-239">attività</span><span class="sxs-lookup"><span data-stu-id="1ba59-239">task</span></span>  |`gulp.task(name[, deps], fn) { }`|<span data-ttu-id="1ba59-240">Il `task` funzione crea un'attività.</span><span class="sxs-lookup"><span data-stu-id="1ba59-240">The `task` function creates a task.</span></span> <span data-ttu-id="1ba59-241">Il `name` parametro definisce il nome dell'attività.</span><span class="sxs-lookup"><span data-stu-id="1ba59-241">The `name` parameter defines the name of the task.</span></span> <span data-ttu-id="1ba59-242">Il `deps` parametro contiene una matrice di attività da completare prima di questa attività viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="1ba59-242">The `deps` parameter contains an array of tasks to be completed before this task runs.</span></span> <span data-ttu-id="1ba59-243">Il `fn` parametro rappresenta una funzione di callback che consente di eseguire le operazioni dell'attività.</span><span class="sxs-lookup"><span data-stu-id="1ba59-243">The `fn` parameter represents a callback function which performs the operations of the task.</span></span>|
|<span data-ttu-id="1ba59-244">Espressioni di controllo</span><span class="sxs-lookup"><span data-stu-id="1ba59-244">watch</span></span> |`gulp.watch(glob [, opts], tasks) { }`|<span data-ttu-id="1ba59-245">Il `watch` funzione consente di monitorare le attività di file e viene eseguito quando si verifica una modifica del file.</span><span class="sxs-lookup"><span data-stu-id="1ba59-245">The `watch` function monitors files and runs tasks when a file change occurs.</span></span> <span data-ttu-id="1ba59-246">Il `glob` parametro è un `string` o `array` che determina quali file da controllare.</span><span class="sxs-lookup"><span data-stu-id="1ba59-246">The `glob` parameter is a `string` or `array` that determines which files to watch.</span></span> <span data-ttu-id="1ba59-247">Il `opts` parametro fornisce la visione di opzioni aggiuntive del file.</span><span class="sxs-lookup"><span data-stu-id="1ba59-247">The `opts` parameter provides additional file watching options.</span></span>|
|<span data-ttu-id="1ba59-248">src</span><span class="sxs-lookup"><span data-stu-id="1ba59-248">src</span></span>   |`gulp.src(globs[, options]) { }`|<span data-ttu-id="1ba59-249">Il `src` funzione fornisce i file che corrispondono al valore/i glob.</span><span class="sxs-lookup"><span data-stu-id="1ba59-249">The `src` function provides files that match the glob value(s).</span></span> <span data-ttu-id="1ba59-250">Il `glob` parametro è un `string` o `array` che determina quali file da leggere.</span><span class="sxs-lookup"><span data-stu-id="1ba59-250">The `glob` parameter is a `string` or `array` that determines which files to read.</span></span> <span data-ttu-id="1ba59-251">Il `options` parametro fornisce opzioni aggiuntive del file.</span><span class="sxs-lookup"><span data-stu-id="1ba59-251">The `options` parameter provides additional file options.</span></span>|
|<span data-ttu-id="1ba59-252">dest</span><span class="sxs-lookup"><span data-stu-id="1ba59-252">dest</span></span>  |`gulp.dest(path[, options]) { }`|<span data-ttu-id="1ba59-253">Il `dest` funzione definisce una posizione in cui è possono scrivere i file.</span><span class="sxs-lookup"><span data-stu-id="1ba59-253">The `dest` function defines a location to which files can be written.</span></span> <span data-ttu-id="1ba59-254">Il `path` parametro è una stringa o una funzione che determina la cartella di destinazione.</span><span class="sxs-lookup"><span data-stu-id="1ba59-254">The `path` parameter is a string or function that determines the destination folder.</span></span> <span data-ttu-id="1ba59-255">Il `options` parametro è un oggetto che specifica le opzioni cartella di output.</span><span class="sxs-lookup"><span data-stu-id="1ba59-255">The `options` parameter is an object that specifies output folder options.</span></span>|

<span data-ttu-id="1ba59-256">Per altre informazioni di riferimento API di Gulp, vedere [Gulp documentazione API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).</span><span class="sxs-lookup"><span data-stu-id="1ba59-256">For additional Gulp API reference information, see [Gulp Docs API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).</span></span>

## <a name="gulp-recipes"></a><span data-ttu-id="1ba59-257">Recipe di gulp</span><span class="sxs-lookup"><span data-stu-id="1ba59-257">Gulp recipes</span></span>

<span data-ttu-id="1ba59-258">La community di Gulp offre Gulp [recipe](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span><span class="sxs-lookup"><span data-stu-id="1ba59-258">The Gulp community provides Gulp [Recipes](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span></span> <span data-ttu-id="1ba59-259">Queste soluzioni sono costituiti da attività Gulp per scenari comuni.</span><span class="sxs-lookup"><span data-stu-id="1ba59-259">These recipes consist of Gulp tasks to address common scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1ba59-260">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="1ba59-260">Additional resources</span></span>

* [<span data-ttu-id="1ba59-261">Documentazione di gulp</span><span class="sxs-lookup"><span data-stu-id="1ba59-261">Gulp documentation</span></span>](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [<span data-ttu-id="1ba59-262">Creazione di bundle e minimizzazione in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1ba59-262">Bundling and minification in ASP.NET Core</span></span>](bundling-and-minification.md)
* [<span data-ttu-id="1ba59-263">Usare Grunt in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1ba59-263">Use Grunt in ASP.NET Core</span></span>](using-grunt.md)
