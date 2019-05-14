---
title: Usare Grunt in ASP.NET Core
author: rick-anderson
description: Usare Grunt in ASP.NET Core
ms.author: riande
ms.date: 05/10/2019
uid: client-side/using-grunt
ms.openlocfilehash: 718a1358c0474711b05bb2c90dc86ec9edacbf1e
ms.sourcegitcommit: 6afe57fb8d9055f88fedb92b16470398c4b9b24a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/14/2019
ms.locfileid: "65610225"
---
# <a name="use-grunt-in-aspnet-core"></a><span data-ttu-id="6d4a9-103">Usare Grunt in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6d4a9-103">Use Grunt in ASP.NET Core</span></span>

<span data-ttu-id="6d4a9-104">Da [riso Noel](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)</span><span class="sxs-lookup"><span data-stu-id="6d4a9-104">By [Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)</span></span>

<span data-ttu-id="6d4a9-105">Grunt è uno strumento di esecuzione attività JavaScript che automatizza la minimizzazione dello script, la compilazione TypeScript, gli strumenti "lint" qualità del codice, pre-processori CSS e praticamente qualsiasi lavoro ingrato ripetitiva che deve eseguire per supportare lo sviluppo di client.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-105">Grunt is a JavaScript task runner that automates script minification, TypeScript compilation, code quality "lint" tools, CSS pre-processors, and just about any repetitive chore that needs doing to support client development.</span></span> <span data-ttu-id="6d4a9-106">Grunt è completamente supportato in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-106">Grunt is fully supported in Visual Studio.</span></span>

<span data-ttu-id="6d4a9-107">Questo esempio Usa un progetto ASP.NET Core vuoto come punto di partenza, in cui viene illustrato come automatizzare il processo di compilazione di client da zero.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-107">This example uses an empty ASP.NET Core project as its starting point, to show how to automate the client build process from scratch.</span></span>

<span data-ttu-id="6d4a9-108">L'esempio completato pulisce la directory di distribuzione di destinazione, combina i file JavaScript, controlli di qualità del codice, condensa contenuto del file JavaScript e distribuisce nella radice dell'applicazione web.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-108">The finished example cleans the target deployment directory, combines JavaScript files, checks code quality, condenses JavaScript file content and deploys to the root of your web application.</span></span> <span data-ttu-id="6d4a9-109">Verranno usati i pacchetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="6d4a9-109">We will use the following packages:</span></span>

* <span data-ttu-id="6d4a9-110">**grunt**: Il pacchetto di strumento di esecuzione attività Grunt.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-110">**grunt**: The Grunt task runner package.</span></span>

* <span data-ttu-id="6d4a9-111">**grunt-contrib-clean**: Un plug-in che consente di rimuovere file o directory.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-111">**grunt-contrib-clean**: A plugin that removes files or directories.</span></span>

* <span data-ttu-id="6d4a9-112">**grunt-contrib-jshint**: Un plug-in che esamina la qualità del codice JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-112">**grunt-contrib-jshint**: A plugin that reviews JavaScript code quality.</span></span>

* <span data-ttu-id="6d4a9-113">**grunt-contrib-concat**: Un plug-in che consente di unire i file in un singolo file.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-113">**grunt-contrib-concat**: A plugin that joins files into a single file.</span></span>

* <span data-ttu-id="6d4a9-114">**grunt-contrib-uglify**: Un plug-in che minimizza JavaScript per ridurre le dimensioni.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-114">**grunt-contrib-uglify**: A plugin that minifies JavaScript to reduce size.</span></span>

* <span data-ttu-id="6d4a9-115">**grunt-contrib-watch**: Un plug-in che verifica la presenza di attività di file.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-115">**grunt-contrib-watch**: A plugin that watches file activity.</span></span>

## <a name="preparing-the-application"></a><span data-ttu-id="6d4a9-116">Preparazione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="6d4a9-116">Preparing the application</span></span>

<span data-ttu-id="6d4a9-117">Per iniziare, configurare una nuova applicazione web vuota e aggiungere i file di esempio di TypeScript.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-117">To begin, set up a new empty web application and add TypeScript example files.</span></span> <span data-ttu-id="6d4a9-118">File TypeScript vengono compilati automaticamente in JavaScript utilizzando le impostazioni di Visual Studio predefinite e saranno nostro materie prime per elaborare l'uso di Grunt.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-118">TypeScript files are automatically compiled into JavaScript using default Visual Studio settings and will be our raw material to process using Grunt.</span></span>

1. <span data-ttu-id="6d4a9-119">In Visual Studio, creare un nuovo `ASP.NET Web Application`.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-119">In Visual Studio, create a new `ASP.NET Web Application`.</span></span>

2. <span data-ttu-id="6d4a9-120">Nel **nuovo progetto ASP.NET** finestra di dialogo selezionare ASP.NET Core **vuota** modello e fare clic sul pulsante OK.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-120">In the **New ASP.NET Project** dialog, select the ASP.NET Core **Empty** template and click the OK button.</span></span>

3. <span data-ttu-id="6d4a9-121">In Esplora soluzioni, esaminare la struttura del progetto.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-121">In the Solution Explorer, review the project structure.</span></span> <span data-ttu-id="6d4a9-122">Il `\src` cartella include vuota `wwwroot` e `Dependencies` nodi.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-122">The `\src` folder includes empty `wwwroot` and `Dependencies` nodes.</span></span>

    ![soluzione web vuoto](using-grunt/_static/grunt-solution-explorer.png)

4. <span data-ttu-id="6d4a9-124">Aggiungere una nuova cartella denominata `TypeScript` alla directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-124">Add a new folder named `TypeScript` to your project directory.</span></span>

5. <span data-ttu-id="6d4a9-125">Prima di aggiungere tutti i file, assicurarsi che Visual Studio include l'opzione ' Compila al salvataggio ' per i file TypeScript selezionati.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-125">Before adding any files, make sure that Visual Studio has the option 'compile on save' for TypeScript files checked.</span></span> <span data-ttu-id="6d4a9-126">Passare a **degli strumenti** > **opzioni** > **Editor di testo** > **Typescript**  >  **Progetto**:</span><span class="sxs-lookup"><span data-stu-id="6d4a9-126">Navigate to **Tools** > **Options** > **Text Editor** > **Typescript** > **Project**:</span></span>

    ![opzioni di impostazione di compilazione automatico dei file TypeScript](using-grunt/_static/typescript-options.png)

6. <span data-ttu-id="6d4a9-128">Fare doppio clic il `TypeScript` directory e selezionare **Aggiungi > Nuovo elemento** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-128">Right-click the `TypeScript` directory and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="6d4a9-129">Selezionare il **file JavaScript** elemento e denominare il file *Tastes.ts* (nota di \*estensione TS).</span><span class="sxs-lookup"><span data-stu-id="6d4a9-129">Select the **JavaScript file** item and name the file *Tastes.ts* (note the \*.ts extension).</span></span> <span data-ttu-id="6d4a9-130">Copiare la riga del codice TypeScript seguente nel file (quando si salva, una nuova *Tastes.js* file verrà visualizzato con l'origine di JavaScript).</span><span class="sxs-lookup"><span data-stu-id="6d4a9-130">Copy the line of TypeScript code below into the file (when you save, a new *Tastes.js* file will appear with the JavaScript source).</span></span>

    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7. <span data-ttu-id="6d4a9-131">Aggiungere un secondo file per il **TypeScript** directory e denominarlo `Food.ts`.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-131">Add a second file to the **TypeScript** directory and name it `Food.ts`.</span></span> <span data-ttu-id="6d4a9-132">Copiare il codice seguente nel file.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-132">Copy the code below into the file.</span></span>

    ```typescript
    class Food {
      constructor(name: string, calories: number) {
        this._name = name;
        this._calories = calories;
      }

      private _name: string;
      get Name() {
        return this._name;
      }

      private _calories: number;
      get Calories() {
        return this._calories;
      }

      private _taste: Tastes;
      get Taste(): Tastes { return this._taste }
      set Taste(value: Tastes) {
        this._taste = value;
      }
    }
    ```

## <a name="configuring-npm"></a><span data-ttu-id="6d4a9-133">Configurazione NPM</span><span class="sxs-lookup"><span data-stu-id="6d4a9-133">Configuring NPM</span></span>

<span data-ttu-id="6d4a9-134">A questo punto, configurare NPM per scaricare grunt e attività di grunt.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-134">Next, configure NPM to download grunt and grunt-tasks.</span></span>

1. <span data-ttu-id="6d4a9-135">In Esplora soluzioni, fare clic sul progetto e selezionare **Aggiungi > Nuovo elemento** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-135">In the Solution Explorer, right-click the project and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="6d4a9-136">Selezionare il **file di configurazione NPM** voce, lasciare il nome predefinito, *package. JSON*, fare clic sul **Aggiungi** pulsante.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-136">Select the **NPM configuration file** item, leave the default name, *package.json*, and click the **Add** button.</span></span>

2. <span data-ttu-id="6d4a9-137">Nel *package. JSON* nel file, all'interno di `devDependencies` oggetto parentesi graffe, immettere "grunt".</span><span class="sxs-lookup"><span data-stu-id="6d4a9-137">In the *package.json* file, inside the `devDependencies` object braces, enter "grunt".</span></span> <span data-ttu-id="6d4a9-138">Selezionare `grunt` Intellisense elenco e premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-138">Select `grunt` from the Intellisense list and press the Enter key.</span></span> <span data-ttu-id="6d4a9-139">Visual Studio verrà racchiudere tra virgolette il nome del pacchetto grunt e aggiungere i due punti.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-139">Visual Studio will quote the grunt package name, and add a colon.</span></span> <span data-ttu-id="6d4a9-140">A destra dei due punti, selezionare la versione stabile più recente del pacchetto dalla parte superiore dell'elenco di Intellisense (premere `Ctrl-Space` se Intellisense non viene visualizzata).</span><span class="sxs-lookup"><span data-stu-id="6d4a9-140">To the right of the colon, select the latest stable version of the package from the top of the Intellisense list (press `Ctrl-Space` if Intellisense doesn't appear).</span></span>

    ![grunt Intellisense](using-grunt/_static/devdependencies-grunt.png)

    > [!NOTE]
    > <span data-ttu-id="6d4a9-142">NPM Usa [versionamento semantico](http://semver.org/) per organizzare le dipendenze.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-142">NPM uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="6d4a9-143">Versionamento semantico, noto anche come SemVer, identifica i pacchetti con lo schema di numerazione \<principale >.\< secondaria >. \<patch >.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-143">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme \<major>.\<minor>.\<patch>.</span></span> <span data-ttu-id="6d4a9-144">IntelliSense semplifica versionamento semantico mostrando solo alcune opzioni comuni.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-144">Intellisense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="6d4a9-145">Il primo elemento nell'elenco di Intellisense (0.4.5 nell'esempio precedente) viene considerato la versione stabile più recente del pacchetto.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-145">The top item in the Intellisense list (0.4.5 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="6d4a9-146">Il simbolo di accento circonflesso (^) corrisponde alla versione principale più recente e la tilde (~) corrisponde alla versione secondaria più recente.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-146">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span> <span data-ttu-id="6d4a9-147">Vedere le [riferimenti ai parser versione di NPM semver](https://www.npmjs.com/package/semver) come guida per l'espressività completa che fornisce SemVer.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-147">See the [NPM semver version parser reference](https://www.npmjs.com/package/semver) as a guide to the full expressivity that SemVer provides.</span></span>

3. <span data-ttu-id="6d4a9-148">Aggiungere altre dipendenze per caricare grunt-contrib -\* pacchetti relativi *pulita*, *jshint*, *concat*, *uglify*e *watch* come illustrato nell'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-148">Add more dependencies to load grunt-contrib-\* packages for *clean*, *jshint*, *concat*, *uglify*, and *watch* as shown in the example below.</span></span> <span data-ttu-id="6d4a9-149">Le versioni non necessario in base all'esempio.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-149">The versions don't need to match the example.</span></span>

    ```json
    "devDependencies": {
      "grunt": "0.4.5",
      "grunt-contrib-clean": "0.6.0",
      "grunt-contrib-jshint": "0.11.0",
      "grunt-contrib-concat": "0.5.1",
      "grunt-contrib-uglify": "0.8.0",
      "grunt-contrib-watch": "0.6.1"
    }
    ```

4. <span data-ttu-id="6d4a9-150">Salvare il *package. JSON* file.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-150">Save the *package.json* file.</span></span>

<span data-ttu-id="6d4a9-151">I pacchetti per ogni elemento devDependencies eseguirà il download, insieme a eventuali file necessari per ogni pacchetto.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-151">The packages for each devDependencies item will download, along with any files that each package requires.</span></span> <span data-ttu-id="6d4a9-152">È possibile trovare i file del pacchetto nel `node_modules` directory abilitando le **Mostra tutti i file** pulsante in Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-152">You can find the package files in the `node_modules` directory by enabling the **Show All Files** button in the Solution Explorer.</span></span>

![node_modules grunt](using-grunt/_static/node-modules.png)

> [!NOTE]
> <span data-ttu-id="6d4a9-154">Se si desidera, è possibile ripristinare manualmente le dipendenze in Esplora soluzioni facendo clic su `Dependencies\NPM` e selezionare il **Ripristina pacchetti** l'opzione di menu.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-154">If you need to, you can manually restore dependencies in Solution Explorer by right-clicking on `Dependencies\NPM` and selecting the **Restore Packages** menu option.</span></span>

![il ripristino dei pacchetti](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a><span data-ttu-id="6d4a9-156">Configurazione di Grunt</span><span class="sxs-lookup"><span data-stu-id="6d4a9-156">Configuring Grunt</span></span>

<span data-ttu-id="6d4a9-157">Grunt viene configurato mediante un manifesto denominato *gruntfile. js* che definisce, carica e registra le attività che possono essere eseguite manualmente o configurate per essere eseguito automaticamente in base sugli eventi in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-157">Grunt is configured using a manifest named *Gruntfile.js* that defines, loads and registers tasks that can be run manually or configured to run automatically based on events in Visual Studio.</span></span>

1. <span data-ttu-id="6d4a9-158">Fare clic sul progetto e selezionare **Aggiungi > Nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-158">Right-click the project and select **Add > New Item**.</span></span> <span data-ttu-id="6d4a9-159">Selezionare il **file di configurazione Grunt** opzione, lasciare il nome predefinito *gruntfile. js*e fare clic sui **Aggiungi** pulsante.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-159">Select the **Grunt Configuration file** option, leave the default name, *Gruntfile.js*, and click the **Add** button.</span></span>

   <span data-ttu-id="6d4a9-160">Il codice iniziale include una definizione di modulo e `grunt.initConfig()` (metodo).</span><span class="sxs-lookup"><span data-stu-id="6d4a9-160">The initial code includes a module definition and the `grunt.initConfig()` method.</span></span> <span data-ttu-id="6d4a9-161">Il `initConfig()` utilizzato per impostare le opzioni per ogni pacchetto, e il resto del modulo verrà caricato e registrare le attività.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-161">The `initConfig()` is used to set options for each package, and the remainder of the module will load and register tasks.</span></span>

   ```javascript
   module.exports = function (grunt) {
     grunt.initConfig({
     });
   };
   ```

2. <span data-ttu-id="6d4a9-162">All'interno di `initConfig()` metodo, aggiungere le opzioni per il `clean` come illustrato nell'esempio di attività *gruntfile. js* sotto.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-162">Inside the `initConfig()` method, add options for the `clean` task as shown in the example *Gruntfile.js* below.</span></span> <span data-ttu-id="6d4a9-163">L'attività clean accetta una matrice di stringhe di directory.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-163">The clean task accepts an array of directory strings.</span></span> <span data-ttu-id="6d4a9-164">Questa operazione rimuove i file da wwwroot/lib e rimuove la directory intera/temp.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-164">This task removes files from wwwroot/lib and removes the entire /temp directory.</span></span>

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

3. <span data-ttu-id="6d4a9-165">Sotto il metodo initConfig(), aggiungere una chiamata a `grunt.loadNpmTasks()`.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-165">Below the initConfig() method, add a call to `grunt.loadNpmTasks()`.</span></span> <span data-ttu-id="6d4a9-166">In questo modo le attività eseguibili da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-166">This will make the task runnable from Visual Studio.</span></span>

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

4. <span data-ttu-id="6d4a9-167">Salvare *gruntfile. js*.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-167">Save *Gruntfile.js*.</span></span> <span data-ttu-id="6d4a9-168">Il file dovrebbe essere simile alla schermata seguente.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-168">The file should look something like the screenshot below.</span></span>

    ![gruntfile iniziale](using-grunt/_static/gruntfile-js-initial.png)

5. <span data-ttu-id="6d4a9-170">Fare doppio clic su *gruntfile. js* e selezionare **Task Runner Explorer** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-170">Right-click *Gruntfile.js* and select **Task Runner Explorer** from the context menu.</span></span> <span data-ttu-id="6d4a9-171">Verrà aperta la finestra di Esplora esecuzione attività.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-171">The Task Runner Explorer window will open.</span></span>

    ![menu di scelta Task runner explorer](using-grunt/_static/task-runner-explorer-menu.png)

6. <span data-ttu-id="6d4a9-173">Verificare che `clean` visualizzato nell'area **attività** in Task Runner Explorer.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-173">Verify that `clean` shows under **Tasks** in the Task Runner Explorer.</span></span>

    ![elenco delle attività Task runner explorer](using-grunt/_static/task-runner-explorer-tasks.png)

7. <span data-ttu-id="6d4a9-175">L'attività clean e scegliere **eseguire** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-175">Right-click the clean task and select **Run** from the context menu.</span></span> <span data-ttu-id="6d4a9-176">Una finestra di comando consente di visualizzare lo stato di avanzamento dell'attività.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-176">A command window displays progress of the task.</span></span>

    ![Task runner explorer eseguire attività di pulizia dell'](using-grunt/_static/task-runner-explorer-run-clean.png)

    > [!NOTE]
    > <span data-ttu-id="6d4a9-178">Non esistono file o directory da pulire ancora.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-178">There are no files or directories to clean yet.</span></span> <span data-ttu-id="6d4a9-179">Se si desidera, è possibile crearle manualmente in Esplora soluzioni e quindi eseguire l'attività clean come test.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-179">If you like, you can manually create them in the Solution Explorer and then run the clean task as a test.</span></span>

8. <span data-ttu-id="6d4a9-180">Nel metodo initConfig(), aggiungere una voce per `concat` usando il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-180">In the initConfig() method, add an entry for `concat` using the code below.</span></span>

    <span data-ttu-id="6d4a9-181">Il `src` matrice proprietà elenca i file per combinare, nell'ordine in cui devono essere combinati.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-181">The `src` property array lists files to combine, in the order that they should be combined.</span></span> <span data-ttu-id="6d4a9-182">Il `dest` proprietà assegna il percorso del file combinato che viene generato.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-182">The `dest` property assigns the path to the combined file that's produced.</span></span>

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```

    > [!NOTE]
    > <span data-ttu-id="6d4a9-183">Il `all` proprietà nel codice precedente è il nome di una destinazione.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-183">The `all` property in the code above is the name of a target.</span></span> <span data-ttu-id="6d4a9-184">Le destinazioni vengono utilizzate in alcune attività di Grunt per consentire più ambienti di compilazione.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-184">Targets are used in some Grunt tasks to allow multiple build environments.</span></span> <span data-ttu-id="6d4a9-185">È possibile visualizzare le destinazioni predefinite usando Intellisense o assegnare il proprio.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-185">You can view the built-in targets using Intellisense or assign your own.</span></span>

9. <span data-ttu-id="6d4a9-186">Aggiungere il `jshint` attività usando il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-186">Add the `jshint` task using the code below.</span></span>

    <span data-ttu-id="6d4a9-187">L'utilità di qualità del codice jshint viene eseguita su tutti i file JavaScript nella directory temp.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-187">The jshint code-quality utility is run against every JavaScript file found in the temp directory.</span></span>

    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > <span data-ttu-id="6d4a9-188">L'opzione "-W069" è un errore generato da jshint quando JavaScript venga utilizzata la sintassi per assegnare una proprietà anziché la notazione del punto, vale a dire a racchiudere tra parentesi quadre `Tastes["Sweet"]` invece di `Tastes.Sweet`.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-188">The option "-W069" is an error produced by jshint when JavaScript uses bracket syntax to assign a property instead of dot notation, i.e. `Tastes["Sweet"]` instead of `Tastes.Sweet`.</span></span> <span data-ttu-id="6d4a9-189">L'opzione Disattiva l'avviso per consentire il resto del processo per continuare.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-189">The option turns off the warning to allow the rest of the process to continue.</span></span>

10. <span data-ttu-id="6d4a9-190">Aggiungere il `uglify` attività usando il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-190">Add the `uglify` task using the code below.</span></span>

    <span data-ttu-id="6d4a9-191">L'attività minimizza la *combined.js* file trovato nella directory temp e crea il file dei risultati in wwwroot/lib seguono la convenzione di denominazione standard  *\<nome file\>. min.js*.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-191">The task minifies the *combined.js* file found in the temp directory and creates the result file in wwwroot/lib following the standard naming convention *\<file name\>.min.js*.</span></span>

    ```javascript
    uglify: {
     all: {
       src: ['temp/combined.js'],
       dest: 'wwwroot/lib/combined.min.js'
     }
    },
    ```

11. <span data-ttu-id="6d4a9-192">Sotto la chiamata grunt.loadNpmTasks() che carica grunt-contrib-pulizia, includere la stessa chiamata per jshint, concat e uglify usando il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-192">Under the call grunt.loadNpmTasks() that loads grunt-contrib-clean, include the same call for jshint, concat and uglify using the code below.</span></span>

    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

12. <span data-ttu-id="6d4a9-193">Salvare *gruntfile. js*.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-193">Save *Gruntfile.js*.</span></span> <span data-ttu-id="6d4a9-194">Il file dovrebbe essere simile all'esempio riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-194">The file should look something like the example below.</span></span>

    ![esempio di file di grunt completo](using-grunt/_static/gruntfile-js-complete.png)

13. <span data-ttu-id="6d4a9-196">Si noti che l'elenco delle attività di Task Runner Explorer include `clean`, `concat`, `jshint` e `uglify` attività.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-196">Notice that the Task Runner Explorer Tasks list includes `clean`, `concat`, `jshint` and `uglify` tasks.</span></span> <span data-ttu-id="6d4a9-197">Eseguire ogni attività nell'ordine e osservare i risultati in Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-197">Run each task in order and observe the results in Solution Explorer.</span></span> <span data-ttu-id="6d4a9-198">Ogni attività dovrebbero funzionare senza errori.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-198">Each task should run without errors.</span></span>

    ![Esplora esecuzione attività eseguire ogni attività](using-grunt/_static/task-runner-explorer-run-each-task.png)

    <span data-ttu-id="6d4a9-200">Crea una nuova attività concat *combined.js* file e lo inserisce nella directory temporanea.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-200">The concat task creates a new *combined.js* file and places it into the temp directory.</span></span> <span data-ttu-id="6d4a9-201">L'attività jshint semplicemente viene eseguito e non genera output.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-201">The jshint task simply runs and doesn't produce output.</span></span> <span data-ttu-id="6d4a9-202">Crea una nuova attività uglify *combined.min.js* file e lo colloca in wwwroot/lib.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-202">The uglify task creates a new *combined.min.js* file and places it into wwwroot/lib.</span></span> <span data-ttu-id="6d4a9-203">Al termine, la soluzione dovrebbe essere qualcosa di simile allo screenshot seguente:</span><span class="sxs-lookup"><span data-stu-id="6d4a9-203">On completion, the solution should look something like the screenshot below:</span></span>

    ![Dopo che tutte le attività di Esplora soluzioni](using-grunt/_static/solution-explorer-after-all-tasks.png)

    > [!NOTE]
    > <span data-ttu-id="6d4a9-205">Per altre informazioni sulle opzioni per ogni pacchetto, visitare [ https://www.npmjs.com/ ](https://www.npmjs.com/) e ricerca il nome del pacchetto nella casella di ricerca nella pagina principale.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-205">For more information on the options for each package, visit [https://www.npmjs.com/](https://www.npmjs.com/) and lookup the package name in the search box on the main page.</span></span> <span data-ttu-id="6d4a9-206">Ad esempio, è possibile cercare il pacchetto grunt-contrib-pulizia per ottenere un collegamento alla documentazione che illustra tutti i relativi parametri.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-206">For example, you can look up the grunt-contrib-clean package to get a documentation link that explains all of its parameters.</span></span>

### <a name="all-together-now"></a><span data-ttu-id="6d4a9-207">Ora è possibile</span><span class="sxs-lookup"><span data-stu-id="6d4a9-207">All together now</span></span>

<span data-ttu-id="6d4a9-208">Uso di Grunt `registerTask()` metodo per eseguire una serie di attività in una determinata sequenza.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-208">Use the Grunt `registerTask()` method to run a series of tasks in a particular sequence.</span></span> <span data-ttu-id="6d4a9-209">Ad esempio, per eseguire l'esempio -> passaggi precedenti nell'ordine pulita concat -> jshint -> uglify, aggiungere il codice seguente al modulo.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-209">For example, to run the example steps above in the order clean -> concat -> jshint -> uglify, add the code below to the module.</span></span> <span data-ttu-id="6d4a9-210">Il codice deve essere aggiunto allo stesso livello delle chiamate loadNpmTasks(), all'esterno di initConfig.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-210">The code should be added to the same level as the loadNpmTasks() calls, outside initConfig.</span></span>

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

<span data-ttu-id="6d4a9-211">La nuova attività viene visualizzato in Esplora esecuzione attività nell'area attività Alias.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-211">The new task shows up in Task Runner Explorer under Alias Tasks.</span></span> <span data-ttu-id="6d4a9-212">È possibile fare doppio clic su ed eseguirlo esattamente come si farebbe altre attività.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-212">You can right-click and run it just as you would other tasks.</span></span> <span data-ttu-id="6d4a9-213">Il `all` verrà eseguita l'operazione `clean`, `concat`, `jshint` e `uglify`, nell'ordine.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-213">The `all` task will run `clean`, `concat`, `jshint` and `uglify`, in order.</span></span>

![attività di grunt alias](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a><span data-ttu-id="6d4a9-215">Controllo per la ricerca di modifiche</span><span class="sxs-lookup"><span data-stu-id="6d4a9-215">Watching for changes</span></span>

<span data-ttu-id="6d4a9-216">Oggetto `watch` attività tiene sotto controllo i file e directory.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-216">A `watch` task keeps an eye on files and directories.</span></span> <span data-ttu-id="6d4a9-217">Se rileva le modifiche, le espressioni di controllo attiva automaticamente le attività.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-217">The watch triggers tasks automatically if it detects changes.</span></span> <span data-ttu-id="6d4a9-218">Aggiungere il codice seguente a initConfig controllare le modifiche a \*file con estensione js nella directory di TypeScript.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-218">Add the code below to initConfig to watch for changes to \*.js files in the TypeScript directory.</span></span> <span data-ttu-id="6d4a9-219">Se viene modificato un file JavaScript, `watch` eseguirà il `all` attività.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-219">If a JavaScript file is changed, `watch` will run the `all` task.</span></span>

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

<span data-ttu-id="6d4a9-220">Aggiungere una chiamata a `loadNpmTasks()` per mostrare il `watch` attività in Task Runner Explorer.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-220">Add a call to `loadNpmTasks()` to show the `watch` task in Task Runner Explorer.</span></span>

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

<span data-ttu-id="6d4a9-221">L'attività di espressione di controllo in Task Runner Explorer e scegliere Esegui dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-221">Right-click the watch task in Task Runner Explorer and select Run from the context menu.</span></span> <span data-ttu-id="6d4a9-222">Nella finestra di comando che mostra l'attività di espressione di controllo in esecuzione verrà visualizzato un "in attesa..." Messaggio.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-222">The command window that shows the watch task running will display a "Waiting…" message.</span></span> <span data-ttu-id="6d4a9-223">Aprire uno dei file TypeScript, aggiungere uno spazio e quindi salvare il file.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-223">Open one of the TypeScript files, add a space, and then save the file.</span></span> <span data-ttu-id="6d4a9-224">Ciò attiva l'attività di espressione di controllo e attivare le altre attività da eseguire in ordine.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-224">This will trigger the watch task and trigger the other tasks to run in order.</span></span> <span data-ttu-id="6d4a9-225">La schermata seguente mostra un esempio di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-225">The screenshot below shows a sample run.</span></span>

![output delle attività in esecuzione](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a><span data-ttu-id="6d4a9-227">Associazione agli eventi di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6d4a9-227">Binding to Visual Studio events</span></span>

<span data-ttu-id="6d4a9-228">A meno che non si desidera avviare manualmente l'attività ogni volta che si lavora in Visual Studio, è possibile associare delle attività **prima di compilare**, **dopo la compilazione**, **Pulisci**, e  **Progetto Open** gli eventi.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-228">Unless you want to manually start your tasks every time you work in Visual Studio, you can bind tasks to **Before Build**, **After Build**, **Clean**, and **Project Open** events.</span></span>

<span data-ttu-id="6d4a9-229">È possibile associare `watch` in modo che venga eseguito ogni volta che viene aperto Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-229">Let’s bind `watch` so that it runs every time Visual Studio opens.</span></span> <span data-ttu-id="6d4a9-230">In Task Runner Explorer, l'attività di espressione di controllo e scegliere **associazioni > Apri progetto** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-230">In Task Runner Explorer, right-click the watch task and select **Bindings > Project Open** from the context menu.</span></span>

![associare un'attività per l'apertura del progetto](using-grunt/_static/bindings-project-open.png)

<span data-ttu-id="6d4a9-232">Scaricare e ricaricare il progetto.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-232">Unload and reload the project.</span></span> <span data-ttu-id="6d4a9-233">Quando il progetto viene caricato anche in questo caso, l'attività di espressione di controllo verrà avviati automaticamente.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-233">When the project loads again, the watch task will start running automatically.</span></span>

## <a name="summary"></a><span data-ttu-id="6d4a9-234">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="6d4a9-234">Summary</span></span>

<span data-ttu-id="6d4a9-235">Grunt è uno strumento di esecuzione di attività potente che può essere utilizzato per automatizzare la maggior parte delle attività di compilazione di client.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-235">Grunt is a powerful task runner that can be used to automate most client-build tasks.</span></span> <span data-ttu-id="6d4a9-236">Monitoraggio prestazioni rete per offrire le funzionalità di integrazione con Visual Studio degli strumenti e i pacchetti, si avvale di grunt.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-236">Grunt leverages NPM to deliver its packages, and features tooling integration with Visual Studio.</span></span> <span data-ttu-id="6d4a9-237">Task Runner Explorer di Visual Studio rileva le modifiche apportate ai file di configurazione e fornisce una comoda interfaccia per eseguire le attività, visualizzare le attività in esecuzione e per associare attività agli eventi di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6d4a9-237">Visual Studio's Task Runner Explorer detects changes to configuration files and provides a convenient interface to run tasks, view running tasks, and bind tasks to Visual Studio events.</span></span>
