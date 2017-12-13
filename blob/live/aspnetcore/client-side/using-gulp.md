---
title: Uso di Gulp in ASP.NET Core
author: rick-anderson
description: Informazioni su come utilizzare Gulp in ASP.NET Core.
keywords: ASP.NET Core, Gulp
ms.author: riande
manager: wpickett
ms.date: 02/28/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-gulp
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 68f6838889cfb830f2c5a1976b3140ae5d94ac25
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-using-gulp-in-aspnet-core"></a><span data-ttu-id="6744a-104">Introduzione all'uso di Gulp in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6744a-104">Introduction to using Gulp in ASP.NET Core</span></span> 

<span data-ttu-id="6744a-105">Da [Erik Reitan](https://github.com/Erikre), [Scott Addie](https://scottaddie.com), [Daniel Roth](https://github.com/danroth27), e [Shayne Boyer](https://twitter.com/spboyer)</span><span class="sxs-lookup"><span data-stu-id="6744a-105">By [Erik Reitan](https://github.com/Erikre), [Scott Addie](https://scottaddie.com), [Daniel Roth](https://github.com/danroth27), and [Shayne Boyer](https://twitter.com/spboyer)</span></span>

<span data-ttu-id="6744a-106">In un'app web moderna tipiche, potrebbe essere il processo di compilazione:</span><span class="sxs-lookup"><span data-stu-id="6744a-106">In a typical modern web app, the build process might:</span></span>

* <span data-ttu-id="6744a-107">Aggregare e minimizzare file CSS e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6744a-107">Bundle and minify JavaScript and CSS files.</span></span>
* <span data-ttu-id="6744a-108">Eseguire gli strumenti per chiamare le operazioni di aggregazione e minimizzazione prima di ogni compilazione.</span><span class="sxs-lookup"><span data-stu-id="6744a-108">Run tools to call the bundling and minification tasks before each build.</span></span>
* <span data-ttu-id="6744a-109">Compilare meno o SASS file CSS.</span><span class="sxs-lookup"><span data-stu-id="6744a-109">Compile LESS or SASS files to CSS.</span></span>
* <span data-ttu-id="6744a-110">Compilare i file di CoffeeScript o TypeScript a JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6744a-110">Compile CoffeeScript or TypeScript files to JavaScript.</span></span>

<span data-ttu-id="6744a-111">Oggetto *runner attività* è uno strumento che consente di automatizzare le attività di sviluppo di routine e altro ancora.</span><span class="sxs-lookup"><span data-stu-id="6744a-111">A *task runner* is a tool which automates these routine development tasks and more.</span></span> <span data-ttu-id="6744a-112">Visual Studio fornisce supporto incorporato per due canali di comuni attività basate su JavaScript: [Gulp](https://gulpjs.com/) e [Grunt](using-grunt.md).</span><span class="sxs-lookup"><span data-stu-id="6744a-112">Visual Studio provides built-in support for two popular JavaScript-based task runners: [Gulp](https://gulpjs.com/) and [Grunt](using-grunt.md).</span></span>

## <a name="gulp"></a><span data-ttu-id="6744a-113">gulp</span><span class="sxs-lookup"><span data-stu-id="6744a-113">Gulp</span></span>

<span data-ttu-id="6744a-114">Gulp è un basate su JavaScript streaming compilazione toolkit per il codice sul lato client.</span><span class="sxs-lookup"><span data-stu-id="6744a-114">Gulp is a JavaScript-based streaming build toolkit for client-side code.</span></span> <span data-ttu-id="6744a-115">Viene usata in genere per i file sul lato client tramite una serie di processi di flusso quando viene generato un evento specifico in un ambiente di compilazione.</span><span class="sxs-lookup"><span data-stu-id="6744a-115">It is commonly used to stream client-side files through a series of processes when a specific event is triggered in a build environment.</span></span> <span data-ttu-id="6744a-116">Ad esempio, Gulp possono essere utilizzati per automatizzare [come aggregare e minimizzazione](bundling-and-minification.md) o la pulizia di un ambiente di sviluppo prima di una nuova compilazione.</span><span class="sxs-lookup"><span data-stu-id="6744a-116">For instance, Gulp can be used to automate [bundling and minification](bundling-and-minification.md) or the cleansing of a development environment before a new build.</span></span>

<span data-ttu-id="6744a-117">Viene definito un set di attività Gulp in *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="6744a-117">A set of Gulp tasks is defined in *gulpfile.js*.</span></span> <span data-ttu-id="6744a-118">Il codice JavaScript seguente include i moduli Gulp e specifica i percorsi di file per fare riferimento all'interno delle attività imminente:</span><span class="sxs-lookup"><span data-stu-id="6744a-118">The following JavaScript includes Gulp modules and specifies file paths to be referenced within the forthcoming tasks:</span></span>

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

<span data-ttu-id="6744a-119">Il codice sopra riportato specifica i moduli di nodo sono necessari.</span><span class="sxs-lookup"><span data-stu-id="6744a-119">The above code specifies which Node modules are required.</span></span> <span data-ttu-id="6744a-120">Il `require` ogni modulo importazioni di funzioni in modo che le attività dipendenti possono utilizzare le funzionalità incluse.</span><span class="sxs-lookup"><span data-stu-id="6744a-120">The `require` function imports each module so that the dependent tasks can utilize their features.</span></span> <span data-ttu-id="6744a-121">Ciascuno dei moduli importati viene assegnato a una variabile.</span><span class="sxs-lookup"><span data-stu-id="6744a-121">Each of the imported modules is assigned to a variable.</span></span> <span data-ttu-id="6744a-122">I moduli possono essere situati in base al nome o percorso.</span><span class="sxs-lookup"><span data-stu-id="6744a-122">The modules can be located either by name or path.</span></span> <span data-ttu-id="6744a-123">In questo esempio, i moduli denominato `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, e `gulp-uglify` vengono recuperati in base al nome.</span><span class="sxs-lookup"><span data-stu-id="6744a-123">In this example, the modules named `gulp`, `rimraf`, `gulp-concat`, `gulp-cssmin`, and `gulp-uglify` are retrieved by name.</span></span> <span data-ttu-id="6744a-124">Inoltre, una serie di percorsi vengono creati in modo che i percorsi di file CSS e JavaScript possono essere riutilizzati e a cui fa riferimento all'interno delle attività.</span><span class="sxs-lookup"><span data-stu-id="6744a-124">Additionally, a series of paths are created so that the locations of CSS and JavaScript files can be reused and referenced within the tasks.</span></span> <span data-ttu-id="6744a-125">Nella tabella seguente vengono fornite le descrizioni dei moduli inclusi in *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="6744a-125">The following table provides descriptions of the modules included in *gulpfile.js*.</span></span>

|<span data-ttu-id="6744a-126">Nome modulo</span><span class="sxs-lookup"><span data-stu-id="6744a-126">Module Name</span></span>|<span data-ttu-id="6744a-127">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6744a-127">Description</span></span>|
|---|---|
|<span data-ttu-id="6744a-128">gulp</span><span class="sxs-lookup"><span data-stu-id="6744a-128">gulp</span></span>|<span data-ttu-id="6744a-129">Il sistema di compilazione flusso Gulp.</span><span class="sxs-lookup"><span data-stu-id="6744a-129">The Gulp streaming build system.</span></span> <span data-ttu-id="6744a-130">Per ulteriori informazioni, vedere [gulp](https://www.npmjs.com/package/gulp).</span><span class="sxs-lookup"><span data-stu-id="6744a-130">For more information, see [gulp](https://www.npmjs.com/package/gulp).</span></span>|
|<span data-ttu-id="6744a-131">rimraf</span><span class="sxs-lookup"><span data-stu-id="6744a-131">rimraf</span></span>|<span data-ttu-id="6744a-132">Un modulo di eliminazione del nodo.</span><span class="sxs-lookup"><span data-stu-id="6744a-132">A Node deletion module.</span></span> <span data-ttu-id="6744a-133">Per ulteriori informazioni, vedere [rimraf](https://www.npmjs.com/package/rimraf).</span><span class="sxs-lookup"><span data-stu-id="6744a-133">For more information, see [rimraf](https://www.npmjs.com/package/rimraf).</span></span>|
|<span data-ttu-id="6744a-134">gulp concat</span><span class="sxs-lookup"><span data-stu-id="6744a-134">gulp-concat</span></span>|<span data-ttu-id="6744a-135">Un modulo che concatena i file in base al carattere di nuova riga del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="6744a-135">A module that concatenates files based on the operating system’s newline character.</span></span> <span data-ttu-id="6744a-136">Per ulteriori informazioni, vedere [gulp concat](https://www.npmjs.com/package/gulp-concat).</span><span class="sxs-lookup"><span data-stu-id="6744a-136">For more information, see [gulp-concat](https://www.npmjs.com/package/gulp-concat).</span></span>|
|<span data-ttu-id="6744a-137">gulp cssmin</span><span class="sxs-lookup"><span data-stu-id="6744a-137">gulp-cssmin</span></span>|<span data-ttu-id="6744a-138">Un modulo che minimizza file CSS.</span><span class="sxs-lookup"><span data-stu-id="6744a-138">A module that minifies CSS files.</span></span> <span data-ttu-id="6744a-139">Per ulteriori informazioni, vedere [gulp cssmin](https://www.npmjs.com/package/gulp-cssmin).</span><span class="sxs-lookup"><span data-stu-id="6744a-139">For more information, see [gulp-cssmin](https://www.npmjs.com/package/gulp-cssmin).</span></span>|
|<span data-ttu-id="6744a-140">uglify gulp</span><span class="sxs-lookup"><span data-stu-id="6744a-140">gulp-uglify</span></span>|<span data-ttu-id="6744a-141">Un modulo che minimizza *. js* file.</span><span class="sxs-lookup"><span data-stu-id="6744a-141">A module that minifies *.js* files.</span></span> <span data-ttu-id="6744a-142">Per ulteriori informazioni, vedere [uglify gulp](https://www.npmjs.com/package/gulp-uglify).</span><span class="sxs-lookup"><span data-stu-id="6744a-142">For more information, see [gulp-uglify](https://www.npmjs.com/package/gulp-uglify).</span></span>|

<span data-ttu-id="6744a-143">Dopo aver importati i moduli necessari, è possibile specificare le attività.</span><span class="sxs-lookup"><span data-stu-id="6744a-143">Once the requisite modules are imported, the tasks can be specified.</span></span> <span data-ttu-id="6744a-144">Di seguito sono riportate le sei attività registrato, rappresentato dal codice seguente:</span><span class="sxs-lookup"><span data-stu-id="6744a-144">Here there are six tasks registered, represented by the following code:</span></span>

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

<span data-ttu-id="6744a-145">Nella tabella seguente viene fornita una spiegazione delle attività specificato nel codice precedente:</span><span class="sxs-lookup"><span data-stu-id="6744a-145">The following table provides an explanation of the tasks specified in the code above:</span></span>

|<span data-ttu-id="6744a-146">Nome attività</span><span class="sxs-lookup"><span data-stu-id="6744a-146">Task Name</span></span>|<span data-ttu-id="6744a-147">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6744a-147">Description</span></span>|
|--- |--- |
|<span data-ttu-id="6744a-148">Pulisci: js</span><span class="sxs-lookup"><span data-stu-id="6744a-148">clean:js</span></span>|<span data-ttu-id="6744a-149">Attività che usa il modulo di eliminazione del nodo rimraf per rimuovere la versione del file site.js minimizzata.</span><span class="sxs-lookup"><span data-stu-id="6744a-149">A task that uses the rimraf Node deletion module to remove the minified version of the site.js file.</span></span>|
|<span data-ttu-id="6744a-150">Pulisci: css</span><span class="sxs-lookup"><span data-stu-id="6744a-150">clean:css</span></span>|<span data-ttu-id="6744a-151">Attività che usa il modulo di eliminazione del nodo rimraf per rimuovere la versione del file site.css minimizzata.</span><span class="sxs-lookup"><span data-stu-id="6744a-151">A task that uses the rimraf Node deletion module to remove the minified version of the site.css file.</span></span>|
|<span data-ttu-id="6744a-152">Pulire</span><span class="sxs-lookup"><span data-stu-id="6744a-152">clean</span></span>|<span data-ttu-id="6744a-153">Un'attività che chiama il `clean:js` attività, aggiungendo il `clean:css` attività.</span><span class="sxs-lookup"><span data-stu-id="6744a-153">A task that calls the `clean:js` task, followed by the `clean:css` task.</span></span>|
|<span data-ttu-id="6744a-154">min:js</span><span class="sxs-lookup"><span data-stu-id="6744a-154">min:js</span></span>|<span data-ttu-id="6744a-155">Attività che minimizza e concatena tutti i file con estensione js all'interno della cartella js.</span><span class="sxs-lookup"><span data-stu-id="6744a-155">A task that minifies and concatenates all .js files within the js folder.</span></span> <span data-ttu-id="6744a-156">I. min.js i file vengono esclusi.</span><span class="sxs-lookup"><span data-stu-id="6744a-156">The .min.js files are excluded.</span></span>|
|<span data-ttu-id="6744a-157">min:CSS</span><span class="sxs-lookup"><span data-stu-id="6744a-157">min:css</span></span>|<span data-ttu-id="6744a-158">Attività che minimizza e concatena tutti i file con estensione CSS all'interno della cartella di css.</span><span class="sxs-lookup"><span data-stu-id="6744a-158">A task that minifies and concatenates all .css files within the css folder.</span></span> <span data-ttu-id="6744a-159">I. min.css i file vengono esclusi.</span><span class="sxs-lookup"><span data-stu-id="6744a-159">The .min.css files are excluded.</span></span>|
|<span data-ttu-id="6744a-160">min</span><span class="sxs-lookup"><span data-stu-id="6744a-160">min</span></span>|<span data-ttu-id="6744a-161">Un'attività che chiama il `min:js` attività, aggiungendo il `min:css` attività.</span><span class="sxs-lookup"><span data-stu-id="6744a-161">A task that calls the `min:js` task, followed by the `min:css` task.</span></span>|

## <a name="running-default-tasks"></a><span data-ttu-id="6744a-162">Esecuzione di attività predefinito</span><span class="sxs-lookup"><span data-stu-id="6744a-162">Running default tasks</span></span>

<span data-ttu-id="6744a-163">Se è già stata creata una nuova app Web, creare un nuovo progetto applicazione Web ASP.NET in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6744a-163">If you haven’t already created a new Web app, create a new ASP.NET Web Application project in Visual Studio.</span></span>

1.  <span data-ttu-id="6744a-164">Aggiungere un nuovo file JavaScript al progetto e denominarlo *gulpfile.js*, quindi copiare il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="6744a-164">Add a new JavaScript file to your project and name it *gulpfile.js*, then copy the following code.</span></span>

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

2.  <span data-ttu-id="6744a-165">Aprire il *package. JSON* file (aggiungere se non esiste) e aggiungere il seguente.</span><span class="sxs-lookup"><span data-stu-id="6744a-165">Open the *package.json* file (add if not there) and add the following.</span></span>

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

3.  <span data-ttu-id="6744a-166">In **Esplora**, fare doppio clic su *gulpfile.js*e selezionare **Task Runner Explorer**.</span><span class="sxs-lookup"><span data-stu-id="6744a-166">In **Solution Explorer**, right-click *gulpfile.js*, and select **Task Runner Explorer**.</span></span>
    
    ![Aprire Task Runner Explorer da Esplora soluzioni](using-gulp/_static/02-SolutionExplorer-TaskRunnerExplorer.png)
    
    <span data-ttu-id="6744a-168">**Esplora esecuzione attività** Mostra un elenco di attività Gulp.</span><span class="sxs-lookup"><span data-stu-id="6744a-168">**Task Runner Explorer** shows the list of Gulp tasks.</span></span> <span data-ttu-id="6744a-169">(Potrebbe essere necessario fare clic su di **aggiornamento** pulsante visualizzato a sinistra del nome del progetto.)</span><span class="sxs-lookup"><span data-stu-id="6744a-169">(You might have to click the **Refresh** button that appears to the left of the project name.)</span></span>
    
    ![Esplora esecuzione attività](using-gulp/_static/03-TaskRunnerExplorer.png)

4.  <span data-ttu-id="6744a-171">Trova di sotto **attività** in **Task Runner Explorer**, fare doppio clic su **pulita**e selezionare **eseguire** dal menu a comparsa.</span><span class="sxs-lookup"><span data-stu-id="6744a-171">Underneath **Tasks** in **Task Runner Explorer**, right-click **clean**, and select **Run** from the pop-up menu.</span></span>

    ![Attività di pulizia dell'Esplora esecuzione attività](using-gulp/_static/04-TaskRunner-clean.png)

    <span data-ttu-id="6744a-173">**Esplora esecuzione attività** creerà una nuova scheda denominata **pulita** ed eseguire l'attività clean come definito *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="6744a-173">**Task Runner Explorer** will create a new tab named **clean** and execute the clean task as it is defined in *gulpfile.js*.</span></span>

5.  <span data-ttu-id="6744a-174">Fare doppio clic su di **pulita** attività, quindi selezionare **associazioni** > **prima di compilare**.</span><span class="sxs-lookup"><span data-stu-id="6744a-174">Right-click the **clean** task, then select **Bindings** > **Before Build**.</span></span>

    ![Associazione BeforeBuild Task Runner Explorer](using-gulp/_static/05-TaskRunner-BeforeBuild.png)

    <span data-ttu-id="6744a-176">Il **prima di compilare** associazione consente di configurare l'attività clean per l'esecuzione automatica prima di ogni compilazione del progetto.</span><span class="sxs-lookup"><span data-stu-id="6744a-176">The **Before Build** binding configures the clean task to run automatically before each build of the project.</span></span>

<span data-ttu-id="6744a-177">Le associazioni è impostare con **Task Runner Explorer** vengono archiviate in forma di un commento nella parte superiore del *gulpfile.js* e diventano effettive solo in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6744a-177">The bindings you set up with **Task Runner Explorer** are stored in the form of a comment at the top of your *gulpfile.js* and are effective only in Visual Studio.</span></span> <span data-ttu-id="6744a-178">Alternativa che non richiede Visual Studio, è possibile configurare l'esecuzione automatica di attività gulp il *csproj* file.</span><span class="sxs-lookup"><span data-stu-id="6744a-178">An alternative that doesn't require Visual Studio is to configure automatic execution of gulp tasks in your *.csproj* file.</span></span> <span data-ttu-id="6744a-179">Ad esempio, contestualizzare il *csproj* file:</span><span class="sxs-lookup"><span data-stu-id="6744a-179">For example, put this in your *.csproj* file:</span></span>

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
  <Exec Command="gulp clean" />
</Target>
```

<span data-ttu-id="6744a-180">Ora l'attività di pulizia viene eseguito quando si esegue il progetto in Visual Studio o da un prompt dei comandi tramite il `dotnet run` comando (eseguire `npm install` prima).</span><span class="sxs-lookup"><span data-stu-id="6744a-180">Now the clean task is executed when you run the project in Visual Studio or from a command prompt using the `dotnet run` command (run `npm install` first).</span></span>

## <a name="defining-and-running-a-new-task"></a><span data-ttu-id="6744a-181">Definizione e l'esecuzione di una nuova attività</span><span class="sxs-lookup"><span data-stu-id="6744a-181">Defining and running a new task</span></span>

<span data-ttu-id="6744a-182">Per definire una nuova attività Gulp, modificare *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="6744a-182">To define a new Gulp task, modify *gulpfile.js*.</span></span>

1.  <span data-ttu-id="6744a-183">Aggiungere il codice JavaScript seguente alla fine di *gulpfile.js*:</span><span class="sxs-lookup"><span data-stu-id="6744a-183">Add the following JavaScript to the end of *gulpfile.js*:</span></span>

    ```javascript
    gulp.task("first", function () {
      console.log('first task! <-----');
    });
    ```

    <span data-ttu-id="6744a-184">Questa operazione è denominata `first`, e visualizza semplicemente una stringa.</span><span class="sxs-lookup"><span data-stu-id="6744a-184">This task is named `first`, and it simply displays a string.</span></span>

2.  <span data-ttu-id="6744a-185">Salvare *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="6744a-185">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="6744a-186">In **Esplora**, fare doppio clic su *gulpfile.js*e selezionare *Task Runner Explorer*.</span><span class="sxs-lookup"><span data-stu-id="6744a-186">In **Solution Explorer**, right-click *gulpfile.js*, and select *Task Runner Explorer*.</span></span>

4.  <span data-ttu-id="6744a-187">In **Task Runner Explorer**, fare doppio clic su **prima**e selezionare **eseguire**.</span><span class="sxs-lookup"><span data-stu-id="6744a-187">In **Task Runner Explorer**, right-click **first**, and select **Run**.</span></span>

    ![Eseguire innanzitutto Task Runner Explorer](using-gulp/_static/06-TaskRunner-First.png)

    <span data-ttu-id="6744a-189">Si noterà che viene visualizzato il testo di output.</span><span class="sxs-lookup"><span data-stu-id="6744a-189">You’ll see that the output text is displayed.</span></span> <span data-ttu-id="6744a-190">Se si è interessati negli esempi in base a uno scenario comune, vedere le recipe Gulp.</span><span class="sxs-lookup"><span data-stu-id="6744a-190">If you are interested in examples based on a common scenario, see Gulp Recipes.</span></span>

## <a name="defining-and-running-tasks-in-a-series"></a><span data-ttu-id="6744a-191">Definizione e l'esecuzione di attività in una serie</span><span class="sxs-lookup"><span data-stu-id="6744a-191">Defining and running tasks in a series</span></span>

<span data-ttu-id="6744a-192">Quando si eseguono più attività, le attività eseguite contemporaneamente per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="6744a-192">When you run multiple tasks, the tasks run concurrently by default.</span></span> <span data-ttu-id="6744a-193">Tuttavia, se è necessario eseguire le attività in un ordine specifico, è necessario specificare quando ogni attività sono stata completata, nonché come quali attività dipendono dal completamento di un'altra attività.</span><span class="sxs-lookup"><span data-stu-id="6744a-193">However, if you need to run tasks in a specific order, you must specify when each task is complete, as well as which tasks depend on the completion of another task.</span></span>

1.  <span data-ttu-id="6744a-194">Per definire una serie di attività da eseguire in ordine, sostituire il `first` attività aggiunte in precedenza nella *gulpfile.js* con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="6744a-194">To define a series of tasks to run in order, replace the `first` task that you added above in *gulpfile.js* with the following:</span></span>

    ```javascript
    gulp.task("series:first", function () {
      console.log('first task! <-----');
    });
 
    gulp.task("series:second", ["series:first"], function () {
      console.log('second task! <-----');
    });
 
    gulp.task("series", ["series:first", "series:second"], function () {});
    ```
 
    <span data-ttu-id="6744a-195">Ora è tre attività: `series:first`, `series:second`, e `series`.</span><span class="sxs-lookup"><span data-stu-id="6744a-195">You now have three tasks: `series:first`, `series:second`, and `series`.</span></span> <span data-ttu-id="6744a-196">Il `series:second` attività include un secondo parametro che specifica una matrice di attività deve essere eseguita e completata prima di `series:second` attività verrà eseguita.</span><span class="sxs-lookup"><span data-stu-id="6744a-196">The `series:second` task includes a second parameter which specifies an array of tasks to be run and completed before the `series:second` task will run.</span></span>  <span data-ttu-id="6744a-197">Come specificato nel codice precedente, solo il `series:first` attività deve essere completata prima di `series:second` attività verrà eseguita.</span><span class="sxs-lookup"><span data-stu-id="6744a-197">As specified in the code above, only the `series:first` task must be completed before the `series:second` task will run.</span></span>

2.  <span data-ttu-id="6744a-198">Salvare *gulpfile.js*.</span><span class="sxs-lookup"><span data-stu-id="6744a-198">Save *gulpfile.js*.</span></span>

3.  <span data-ttu-id="6744a-199">In **Esplora**, fare doppio clic su *gulpfile.js* e selezionare **Task Runner Explorer** se non è già aperto.</span><span class="sxs-lookup"><span data-stu-id="6744a-199">In **Solution Explorer**, right-click *gulpfile.js* and select **Task Runner Explorer** if it isn’t already open.</span></span>

4.  <span data-ttu-id="6744a-200">In **Task Runner Explorer**, fare doppio clic su **serie** e selezionare **eseguire**.</span><span class="sxs-lookup"><span data-stu-id="6744a-200">In **Task Runner Explorer**, right-click **series** and select **Run**.</span></span>

    ![Eseguire attività di serie Task Runner Explorer](using-gulp/_static/07-TaskRunner-Series.png)

## <a name="intellisense"></a><span data-ttu-id="6744a-202">IntelliSense</span><span class="sxs-lookup"><span data-stu-id="6744a-202">IntelliSense</span></span>

<span data-ttu-id="6744a-203">IntelliSense offre il completamento del codice, descrizioni dei parametri e altre funzionalità per la produttività e ridurre gli errori.</span><span class="sxs-lookup"><span data-stu-id="6744a-203">IntelliSense provides code completion, parameter descriptions, and other features to boost productivity and to decrease errors.</span></span> <span data-ttu-id="6744a-204">Attività gulp scritte in JavaScript. Pertanto, IntelliSense può fornire assistenza durante lo sviluppo.</span><span class="sxs-lookup"><span data-stu-id="6744a-204">Gulp tasks are written in JavaScript; therefore, IntelliSense can provide assistance while developing.</span></span> <span data-ttu-id="6744a-205">Mentre si lavora con JavaScript, IntelliSense Elenca gli oggetti, funzioni, proprietà e i parametri che sono disponibili in base al contesto corrente.</span><span class="sxs-lookup"><span data-stu-id="6744a-205">As you work with JavaScript, IntelliSense lists the objects, functions, properties, and parameters that are available based on your current context.</span></span> <span data-ttu-id="6744a-206">Selezionare un'opzione di codifica dall'elenco popup fornito da IntelliSense per completare il codice.</span><span class="sxs-lookup"><span data-stu-id="6744a-206">Select a coding option from the pop-up list provided by IntelliSense to complete the code.</span></span>

![gulp IntelliSense](using-gulp/_static/08-IntelliSense.png)

<span data-ttu-id="6744a-208">Per ulteriori informazioni su IntelliSense, vedere [JavaScript IntelliSense](https://docs.microsoft.com/visualstudio/ide/javascript-intellisense).</span><span class="sxs-lookup"><span data-stu-id="6744a-208">For more information about IntelliSense, see [JavaScript IntelliSense](https://docs.microsoft.com/visualstudio/ide/javascript-intellisense).</span></span>

## <a name="development-staging-and-production-environments"></a><span data-ttu-id="6744a-209">Ambienti di sviluppo, gestione temporanea e produzione</span><span class="sxs-lookup"><span data-stu-id="6744a-209">Development, staging, and production environments</span></span>

<span data-ttu-id="6744a-210">Quando Gulp viene utilizzata per ottimizzare i file sul lato client per la gestione temporanea e produzione, i file elaborati vengono salvati in un percorso locale di gestione temporanea e produzione.</span><span class="sxs-lookup"><span data-stu-id="6744a-210">When Gulp is used to optimize client-side files for staging and production, the processed files are saved to a local staging and production location.</span></span> <span data-ttu-id="6744a-211">Il *layout. cshtml* file Usa il **ambiente** tag helper per fornire due diverse versioni di file CSS.</span><span class="sxs-lookup"><span data-stu-id="6744a-211">The *_Layout.cshtml* file uses the **environment** tag helper to provide two different versions of CSS files.</span></span> <span data-ttu-id="6744a-212">È una versione di file CSS per lo sviluppo e l'altra versione è ottimizzata per la gestione temporanea e produzione.</span><span class="sxs-lookup"><span data-stu-id="6744a-212">One version of CSS files is for development and the other version is optimized for both staging and production.</span></span> <span data-ttu-id="6744a-213">In Visual Studio 2017, quando si modifica il **ASPNETCORE_ENVIRONMENT** variabile di ambiente `Production`, Visual Studio verrà compilato l'app Web e un collegamento ai file CSS ridotta a icona.</span><span class="sxs-lookup"><span data-stu-id="6744a-213">In Visual Studio 2017, when you change the **ASPNETCORE_ENVIRONMENT** environment variable to `Production`, Visual Studio will build the Web app and link to the minimized CSS files.</span></span> <span data-ttu-id="6744a-214">Il markup seguente mostra il **ambiente** gli helper contenente il tag di collegamento al tag di `Development` CSS file e il minimizzata `Staging, Production` file CSS.</span><span class="sxs-lookup"><span data-stu-id="6744a-214">The following markup shows the **environment** tag helpers containing link tags to the `Development` CSS files and the minified `Staging, Production` CSS files.</span></span>

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

## <a name="switching-between-environments"></a><span data-ttu-id="6744a-215">Il passaggio tra ambienti</span><span class="sxs-lookup"><span data-stu-id="6744a-215">Switching between environments</span></span>

<span data-ttu-id="6744a-216">Per passare tra la compilazione per ambienti diversi, modificare il **ASPNETCORE_ENVIRONMENT** valore della variabile di ambiente.</span><span class="sxs-lookup"><span data-stu-id="6744a-216">To switch between compiling for different environments, modify the **ASPNETCORE_ENVIRONMENT** environment variable's value.</span></span>

1.  <span data-ttu-id="6744a-217">In **Task Runner Explorer**, verificare che il **min** attività è stata impostata per l'esecuzione **prima di compilare**.</span><span class="sxs-lookup"><span data-stu-id="6744a-217">In **Task Runner Explorer**, verify that the **min** task has been set to run **Before Build**.</span></span>

2.  <span data-ttu-id="6744a-218">In **Esplora**, il nome del progetto e scegliere **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="6744a-218">In **Solution Explorer**, right-click the project name and select **Properties**.</span></span>

    <span data-ttu-id="6744a-219">Viene visualizzata la finestra delle proprietà per l'app Web.</span><span class="sxs-lookup"><span data-stu-id="6744a-219">The property sheet for the Web app is displayed.</span></span>

3.  <span data-ttu-id="6744a-220">Fare clic sulla scheda **Debug**.</span><span class="sxs-lookup"><span data-stu-id="6744a-220">Click the **Debug** tab.</span></span>

4.  <span data-ttu-id="6744a-221">Impostare il valore della **: l'ambiente di Hosting** variabile di ambiente `Production`.</span><span class="sxs-lookup"><span data-stu-id="6744a-221">Set the value of the **Hosting:Environment** environment variable to `Production`.</span></span>

5.  <span data-ttu-id="6744a-222">Premere **F5** per eseguire l'applicazione in un browser.</span><span class="sxs-lookup"><span data-stu-id="6744a-222">Press **F5** to run the application in a browser.</span></span>

6.  <span data-ttu-id="6744a-223">Nella finestra del browser, la pagina e scegliere **Visualizza origine** per visualizzare il codice HTML della pagina.</span><span class="sxs-lookup"><span data-stu-id="6744a-223">In the browser window, right-click the page and select **View Source** to view the HTML for the page.</span></span>

    <span data-ttu-id="6744a-224">Si noti che i collegamenti di foglio di stile puntano ai file CSS minimizzati.</span><span class="sxs-lookup"><span data-stu-id="6744a-224">Notice that the stylesheet links point to the minified CSS files.</span></span>

7.  <span data-ttu-id="6744a-225">Chiudere il browser per arrestare l'applicazione Web.</span><span class="sxs-lookup"><span data-stu-id="6744a-225">Close the browser to stop the Web app.</span></span>

8.  <span data-ttu-id="6744a-226">In Visual Studio, tornare alla finestra delle proprietà per l'app Web e modificare il **: l'ambiente di Hosting** variabile di ambiente nuovamente a `Development`.</span><span class="sxs-lookup"><span data-stu-id="6744a-226">In Visual Studio, return to the property sheet for the Web app and change the **Hosting:Environment** environment variable back to `Development`.</span></span>

9.  <span data-ttu-id="6744a-227">Premere **F5** a eseguire nuovamente l'applicazione in un browser.</span><span class="sxs-lookup"><span data-stu-id="6744a-227">Press **F5** to run the application in a browser again.</span></span>

10. <span data-ttu-id="6744a-228">Nella finestra del browser, la pagina e scegliere **Visualizza origine** per visualizzare il codice HTML della pagina.</span><span class="sxs-lookup"><span data-stu-id="6744a-228">In the browser window, right-click the page and select **View Source** to see the HTML for the page.</span></span>

    <span data-ttu-id="6744a-229">Si noti che i collegamenti di foglio di stile puntano alle versioni dei file CSS non minimizzate.</span><span class="sxs-lookup"><span data-stu-id="6744a-229">Notice that the stylesheet links point to the unminified versions of the CSS files.</span></span>

<span data-ttu-id="6744a-230">Per ulteriori informazioni relative agli ambienti di ASP.NET Core, vedere [utilizzo di più ambienti](../fundamentals/environments.md).</span><span class="sxs-lookup"><span data-stu-id="6744a-230">For more information related to environments in ASP.NET Core, see [Working with Multiple Environments](../fundamentals/environments.md).</span></span>

## <a name="task-and-module-details"></a><span data-ttu-id="6744a-231">Dettagli attività e il modulo</span><span class="sxs-lookup"><span data-stu-id="6744a-231">Task and module details</span></span>

<span data-ttu-id="6744a-232">Un'attività Gulp è registrata con un nome di funzione.</span><span class="sxs-lookup"><span data-stu-id="6744a-232">A Gulp task is registered with a function name.</span></span>  <span data-ttu-id="6744a-233">Se è necessario eseguire altre attività prima dell'attività corrente, è possibile specificare le dipendenze.</span><span class="sxs-lookup"><span data-stu-id="6744a-233">You can specify dependencies if other tasks must run before the current task.</span></span> <span data-ttu-id="6744a-234">Funzioni aggiuntive consentono di eseguire e controllare le attività Gulp, nonché di impostare l'origine (*src*) e di destinazione (*dest*) dei file da modificare.</span><span class="sxs-lookup"><span data-stu-id="6744a-234">Additional functions allow you to run and watch the Gulp tasks, as well as set the source (*src*) and destination (*dest*) of the files being modified.</span></span> <span data-ttu-id="6744a-235">Le funzioni API Gulp principali sono i seguenti:</span><span class="sxs-lookup"><span data-stu-id="6744a-235">The following are the primary Gulp API functions:</span></span>

|<span data-ttu-id="6744a-236">Gulp (funzione)</span><span class="sxs-lookup"><span data-stu-id="6744a-236">Gulp Function</span></span>|<span data-ttu-id="6744a-237">Sintassi</span><span class="sxs-lookup"><span data-stu-id="6744a-237">Syntax</span></span>|<span data-ttu-id="6744a-238">Descrizione</span><span class="sxs-lookup"><span data-stu-id="6744a-238">Description</span></span>|
|---   |--- |--- |
|<span data-ttu-id="6744a-239">attività</span><span class="sxs-lookup"><span data-stu-id="6744a-239">task</span></span>  |`gulp.task(name[, deps], fn) { }`|<span data-ttu-id="6744a-240">Il `task` funzione crea un'attività.</span><span class="sxs-lookup"><span data-stu-id="6744a-240">The `task` function creates a task.</span></span> <span data-ttu-id="6744a-241">Il `name` parametro definisce il nome dell'attività.</span><span class="sxs-lookup"><span data-stu-id="6744a-241">The `name` parameter defines the name of the task.</span></span> <span data-ttu-id="6744a-242">Il `deps` parametro contiene una matrice di attività da completare prima dell'esecuzione di questa attività.</span><span class="sxs-lookup"><span data-stu-id="6744a-242">The `deps` parameter contains an array of tasks to be completed before this task runs.</span></span> <span data-ttu-id="6744a-243">Il `fn` parametro rappresenta una funzione di callback che esegue le operazioni dell'attività.</span><span class="sxs-lookup"><span data-stu-id="6744a-243">The `fn` parameter represents a callback function which performs the operations of the task.</span></span>|
|<span data-ttu-id="6744a-244">Espressioni di controllo</span><span class="sxs-lookup"><span data-stu-id="6744a-244">watch</span></span> |`gulp.watch(glob [, opts], tasks) { }`|<span data-ttu-id="6744a-245">Il `watch` funzione consente di monitorare le attività di file e viene eseguito quando si verifica una modifica di file.</span><span class="sxs-lookup"><span data-stu-id="6744a-245">The `watch` function monitors files and runs tasks when a file change occurs.</span></span> <span data-ttu-id="6744a-246">Il `glob` parametro è un `string` o `array` che determina quali sono i file da controllare.</span><span class="sxs-lookup"><span data-stu-id="6744a-246">The `glob` parameter is a `string` or `array` that determines which files to watch.</span></span> <span data-ttu-id="6744a-247">Il `opts` parametro fornisce controllo Opzioni aggiuntive del file.</span><span class="sxs-lookup"><span data-stu-id="6744a-247">The `opts` parameter provides additional file watching options.</span></span>|
|<span data-ttu-id="6744a-248">src</span><span class="sxs-lookup"><span data-stu-id="6744a-248">src</span></span>   |`gulp.src(globs[, options]) { }`|<span data-ttu-id="6744a-249">Il `src` funzione fornisce i file che corrispondono ai valori glob.</span><span class="sxs-lookup"><span data-stu-id="6744a-249">The `src` function provides files that match the glob value(s).</span></span> <span data-ttu-id="6744a-250">Il `glob` parametro è un `string` o `array` che determina quali sono i file da leggere.</span><span class="sxs-lookup"><span data-stu-id="6744a-250">The `glob` parameter is a `string` or `array` that determines which files to read.</span></span> <span data-ttu-id="6744a-251">Il `options` parametro fornisce opzioni aggiuntive del file.</span><span class="sxs-lookup"><span data-stu-id="6744a-251">The `options` parameter provides additional file options.</span></span>|
|<span data-ttu-id="6744a-252">distruttore</span><span class="sxs-lookup"><span data-stu-id="6744a-252">dest</span></span>  |`gulp.dest(path[, options]) { }`|<span data-ttu-id="6744a-253">Il `dest` funzione definisce una posizione in cui è possano scrivere file.</span><span class="sxs-lookup"><span data-stu-id="6744a-253">The `dest` function defines a location to which files can be written.</span></span> <span data-ttu-id="6744a-254">Il `path` parametro è una stringa o una funzione che determina la cartella di destinazione.</span><span class="sxs-lookup"><span data-stu-id="6744a-254">The `path` parameter is a string or function that determines the destination folder.</span></span> <span data-ttu-id="6744a-255">Il `options` parametro è un oggetto che specifica le opzioni di cartella di output.</span><span class="sxs-lookup"><span data-stu-id="6744a-255">The `options` parameter is an object that specifies output folder options.</span></span>|

<span data-ttu-id="6744a-256">Per ulteriori informazioni di riferimento Gulp API, vedere [Gulp documenti API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).</span><span class="sxs-lookup"><span data-stu-id="6744a-256">For additional Gulp API reference information, see [Gulp Docs API](https://github.com/gulpjs/gulp/blob/master/docs/API.md).</span></span>

## <a name="gulp-recipes"></a><span data-ttu-id="6744a-257">Gulp ricette</span><span class="sxs-lookup"><span data-stu-id="6744a-257">Gulp recipes</span></span>

<span data-ttu-id="6744a-258">La Comunità Gulp fornisce Gulp [ricette](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span><span class="sxs-lookup"><span data-stu-id="6744a-258">The Gulp community provides Gulp [recipes](https://github.com/gulpjs/gulp/blob/master/docs/recipes/README.md).</span></span> <span data-ttu-id="6744a-259">Queste soluzioni sono costituiti da attività Gulp per scenari comuni.</span><span class="sxs-lookup"><span data-stu-id="6744a-259">These recipes consist of Gulp tasks to address common scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6744a-260">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="6744a-260">Additional resources</span></span>

* [<span data-ttu-id="6744a-261">Documentazione gulp</span><span class="sxs-lookup"><span data-stu-id="6744a-261">Gulp documentation</span></span>](https://github.com/gulpjs/gulp/blob/master/docs/README.md)
* [<span data-ttu-id="6744a-262">Come aggregare e riduzione in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6744a-262">Bundling and minification in ASP.NET Core</span></span>](bundling-and-minification.md)
* [<span data-ttu-id="6744a-263">Utilizzo di Grunt in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6744a-263">Using Grunt in ASP.NET Core</span></span>](using-grunt.md)
