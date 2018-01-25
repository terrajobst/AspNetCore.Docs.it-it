---
title: Utilizzo di Grunt in ASP.NET Core
author: rick-anderson
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-grunt
ms.openlocfilehash: 527373829754757e52ab84b64e04702d649e9062
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="using-grunt-in-aspnet-core"></a><span data-ttu-id="f9ddc-102">Utilizzo di Grunt in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f9ddc-102">Using Grunt in ASP.NET Core</span></span> 

<span data-ttu-id="f9ddc-103">Da [Noel riso](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)</span><span class="sxs-lookup"><span data-stu-id="f9ddc-103">By [Noel Rice](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)</span></span>

<span data-ttu-id="f9ddc-104">Grunt è un'esecuzione attività JavaScript che consente di automatizzare minimizzazione script, la compilazione di TypeScript, gli strumenti "lint" qualità del codice, pre-processori di CSS e praticamente alcun ricorrenti complessa che deve eseguire per supportare lo sviluppo di client.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-104">Grunt is a JavaScript task runner that automates script minification, TypeScript compilation, code quality "lint" tools, CSS pre-processors, and just about any repetitive chore that needs doing to support client development.</span></span> <span data-ttu-id="f9ddc-105">Grunt è completamente supportato in Visual Studio, anche se i modelli di progetto ASP.NET utilizzano Gulp per impostazione predefinita (vedere [utilizzando Gulp](using-gulp.md)).</span><span class="sxs-lookup"><span data-stu-id="f9ddc-105">Grunt is fully supported in Visual Studio, though the ASP.NET project templates use Gulp by default (see [Using Gulp](using-gulp.md)).</span></span>

<span data-ttu-id="f9ddc-106">In questo esempio utilizza un progetto ASP.NET Core vuoto come punto di partenza, per mostrare come automatizzare il processo di compilazione del client da zero.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-106">This example uses an empty ASP.NET Core project as its starting point, to show how to automate the client build process from scratch.</span></span>

<span data-ttu-id="f9ddc-107">Nell'esempio di aver pulisce la directory di distribuzione di destinazione, combina i file JavaScript, controlli di qualità del codice, condensa contenuto di un file JavaScript e distribuisce nella radice dell'applicazione web.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-107">The finished example cleans the target deployment directory, combines JavaScript files, checks code quality, condenses JavaScript file content and deploys to the root of your web application.</span></span> <span data-ttu-id="f9ddc-108">Si utilizzerà i pacchetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="f9ddc-108">We will use the following packages:</span></span>

* <span data-ttu-id="f9ddc-109">**grunt**: Grunt l'attività runner il pacchetto.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-109">**grunt**: The Grunt task runner package.</span></span>

* <span data-ttu-id="f9ddc-110">**grunt-pensionistici pulire**: un plug-in che rimuove i file o directory.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-110">**grunt-contrib-clean**: A plugin that removes files or directories.</span></span>

* <span data-ttu-id="f9ddc-111">**grunt-pensionistici-jshint**: un plug-in che esamina la qualità del codice JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-111">**grunt-contrib-jshint**: A plugin that reviews JavaScript code quality.</span></span>

* <span data-ttu-id="f9ddc-112">**grunt-pensionistici-concat**: un plug-in che unisce i file in un singolo file.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-112">**grunt-contrib-concat**: A plugin that joins files into a single file.</span></span>

* <span data-ttu-id="f9ddc-113">**grunt-pensionistici uglify**: un plug-in che minimizza JavaScript per ridurre le dimensioni.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-113">**grunt-contrib-uglify**: A plugin that minifies JavaScript to reduce size.</span></span>

* <span data-ttu-id="f9ddc-114">**grunt-pensionistici-watch**: un plug-in che esegue attività di file.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-114">**grunt-contrib-watch**: A plugin that watches file activity.</span></span>

## <a name="preparing-the-application"></a><span data-ttu-id="f9ddc-115">Preparazione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="f9ddc-115">Preparing the application</span></span>

<span data-ttu-id="f9ddc-116">Per iniziare, impostare una nuova applicazione web vuota e aggiungere i file di esempio TypeScript.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-116">To begin, set up a new empty web application and add TypeScript example files.</span></span> <span data-ttu-id="f9ddc-117">I file typeScript vengono compilati automaticamente in JavaScript utilizzando le impostazioni di Visual Studio predefinito e saranno il nostro materie prime per elaborare utilizzando Grunt.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-117">TypeScript files are automatically compiled into JavaScript using default Visual Studio settings and will be our raw material to process using Grunt.</span></span>

1.  <span data-ttu-id="f9ddc-118">In Visual Studio, creare un nuovo `ASP.NET Web Application`.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-118">In Visual Studio, create a new `ASP.NET Web Application`.</span></span>

2.  <span data-ttu-id="f9ddc-119">Nel **nuovo progetto ASP.NET** finestra di dialogo, selezionare ASP.NET Core **vuoto** modello e fare clic sul pulsante OK.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-119">In the **New ASP.NET Project** dialog, select the ASP.NET Core **Empty** template and click the OK button.</span></span>

3.  <span data-ttu-id="f9ddc-120">In Esplora soluzioni, esaminare la struttura del progetto.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-120">In the Solution Explorer, review the project structure.</span></span> <span data-ttu-id="f9ddc-121">Il `\src` cartella include vuoto `wwwroot` e `Dependencies` nodi.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-121">The `\src` folder includes empty `wwwroot` and `Dependencies` nodes.</span></span>

    ![soluzione web vuoto](using-grunt/_static/grunt-solution-explorer.png)

4.  <span data-ttu-id="f9ddc-123">Aggiungere una nuova cartella denominata `TypeScript` alla directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-123">Add a new folder named `TypeScript` to your project directory.</span></span>

5.  <span data-ttu-id="f9ddc-124">Prima di aggiungere tutti i file, assicurarsi che Visual Studio è l'opzione ' compilare al salvataggio ' per i file TypeScript selezionati.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-124">Before adding any files, let’s make sure that Visual Studio has the option 'compile on save' for TypeScript files checked.</span></span> <span data-ttu-id="f9ddc-125">*Strumenti > Opzioni > Editor di testo > Typescript > progetto*</span><span class="sxs-lookup"><span data-stu-id="f9ddc-125">*Tools > Options > Text Editor > Typescript > Project*</span></span>

    ![opzioni di impostazione compliation automatica dei file TypeScript](using-grunt/_static/typescript-options.png)

6.  <span data-ttu-id="f9ddc-127">Fare doppio clic su di `TypeScript` directory e selezionare **Aggiungi > Nuovo elemento** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-127">Right-click the `TypeScript` directory and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="f9ddc-128">Selezionare il **file JavaScript** elemento e denominare il file *Tastes.ts* (si noti il \*estensione TS).</span><span class="sxs-lookup"><span data-stu-id="f9ddc-128">Select the **JavaScript file** item and name the file *Tastes.ts* (note the \*.ts extension).</span></span> <span data-ttu-id="f9ddc-129">Copiare la riga di codice TypeScript seguente nel file (quando si salva, un nuovo *Tastes.js* file verrà visualizzato con l'origine di JavaScript).</span><span class="sxs-lookup"><span data-stu-id="f9ddc-129">Copy the line of TypeScript code below into the file (when you save, a new *Tastes.js* file will appear with the JavaScript source).</span></span>
    
    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7.  <span data-ttu-id="f9ddc-130">Aggiungere un secondo file per il **TypeScript** directory e denominarla `Food.ts`.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-130">Add a second file to the **TypeScript** directory and name it `Food.ts`.</span></span> <span data-ttu-id="f9ddc-131">Copiare il codice seguente nel file.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-131">Copy the code below into the file.</span></span>

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

## <a name="configuring-npm"></a><span data-ttu-id="f9ddc-132">Configurazione di NPM</span><span class="sxs-lookup"><span data-stu-id="f9ddc-132">Configuring NPM</span></span>

<span data-ttu-id="f9ddc-133">Configurare quindi NPM per scaricare grunt e grunt-attività.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-133">Next, configure NPM to download grunt and grunt-tasks.</span></span>

1. <span data-ttu-id="f9ddc-134">In Esplora soluzioni, fare clic sul progetto e selezionare **Aggiungi > Nuovo elemento** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-134">In the Solution Explorer, right-click the project and select **Add > New Item** from the context menu.</span></span> <span data-ttu-id="f9ddc-135">Selezionare il **file di configurazione NPM** voce, lasciare il nome predefinito, *package. JSON*, fare clic su di **Aggiungi** pulsante.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-135">Select the **NPM configuration file** item, leave the default name, *package.json*, and click the **Add** button.</span></span>

2. <span data-ttu-id="f9ddc-136">Nel *package. JSON* file, all'interno di `devDependencies` oggetto tra parentesi graffe, immettere "grunt".</span><span class="sxs-lookup"><span data-stu-id="f9ddc-136">In the *package.json* file, inside the `devDependencies` object braces, enter "grunt".</span></span> <span data-ttu-id="f9ddc-137">Selezionare `grunt` elenco Intellisense e premere INVIO.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-137">Select `grunt` from the Intellisense list and press the Enter key.</span></span> <span data-ttu-id="f9ddc-138">Visual Studio verrà offerta il nome del pacchetto grunt e aggiungere i due punti.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-138">Visual Studio will quote the grunt package name, and add a colon.</span></span> <span data-ttu-id="f9ddc-139">A destra dei due punti, selezionare la versione stabile più recente del pacchetto dalla parte superiore dell'elenco di Intellisense (premere `Ctrl-Space` se Intellisense non è presente).</span><span class="sxs-lookup"><span data-stu-id="f9ddc-139">To the right of the colon, select the latest stable version of the package from the top of the Intellisense list (press `Ctrl-Space` if Intellisense doesn't appear).</span></span>

    ![grun Intellisense](using-grunt/_static/devdependencies-grunt.png)
    
    > [!NOTE]
    > <span data-ttu-id="f9ddc-141">Usa NPM [controllo delle versioni semantico](http://semver.org/) per organizzare le dipendenze.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-141">NPM uses [semantic versioning](http://semver.org/) to organize dependencies.</span></span> <span data-ttu-id="f9ddc-142">Controllo delle versioni semantico, noto anche come SemVer, identifica i pacchetti con lo schema di numerazione <major>.<minor>. <patch>. IntelliSense semplifica il controllo delle versioni semantico mostrando solo alcune opzioni comuni.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-142">Semantic versioning, also known as SemVer, identifies packages with the numbering scheme <major>.<minor>.<patch>. Intellisense simplifies semantic versioning by showing only a few common choices.</span></span> <span data-ttu-id="f9ddc-143">Il primo elemento nell'elenco di Intellisense (0.4.5 nell'esempio precedente) viene considerato l'ultima versione stabile del pacchetto.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-143">The top item in the Intellisense list (0.4.5 in the example above) is considered the latest stable version of the package.</span></span> <span data-ttu-id="f9ddc-144">Il simbolo di accento circonflesso (^) corrisponda alla versione principale più recente e la tilde (~) corrisponda alla versione secondaria più recente.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-144">The caret (^) symbol matches the most recent major version and the tilde (~) matches the most recent minor version.</span></span> <span data-ttu-id="f9ddc-145">Vedere il [riferimento parser versione NPM semver](https://www.npmjs.com/package/semver) come guida per l'espressività completa che fornisce SemVer.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-145">See the [NPM semver version parser reference](https://www.npmjs.com/package/semver) as a guide to the full expressivity that SemVer provides.</span></span>

3. <span data-ttu-id="f9ddc-146">Aggiungere altre dipendenze di caricare grunt-pensionistici -\* pacchetti per *pulita*, *jshint*, *concat*, *uglify*e *espressioni di controllo* come illustrato nell'esempio riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-146">Add more dependencies to load grunt-contrib-\* packages for *clean*, *jshint*, *concat*, *uglify*, and *watch* as shown in the example below.</span></span> <span data-ttu-id="f9ddc-147">Le versioni non necessario in base all'esempio.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-147">The versions don't need to match the example.</span></span>

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

4. <span data-ttu-id="f9ddc-148">Salvare il *package. JSON* file.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-148">Save the *package.json* file.</span></span>

<span data-ttu-id="f9ddc-149">I pacchetti per ogni elemento devDependencies eseguirà il download, insieme a tutti i file che richiede di ogni pacchetto.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-149">The packages for each devDependencies item will download, along with any files that each package requires.</span></span> <span data-ttu-id="f9ddc-150">È possibile trovare i file del pacchetto nel `node_modules` directory abilitando il **Mostra tutti i file** pulsante in Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-150">You can find the package files in the `node_modules` directory by enabling the **Show All Files** button in the Solution Explorer.</span></span>

![grunt node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> <span data-ttu-id="f9ddc-152">Se si desidera, è possibile ripristinare manualmente le dipendenze in Esplora soluzioni facendo clic su `Dependencies\NPM` e selezionando il **pacchetti ripristinare** opzione di menu.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-152">If you need to, you can manually restore dependencies in Solution Explorer by right-clicking on `Dependencies\NPM` and selecting the **Restore Packages** menu option.</span></span>

![ripristino dei pacchetti](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a><span data-ttu-id="f9ddc-154">Configurazione Grunt</span><span class="sxs-lookup"><span data-stu-id="f9ddc-154">Configuring Grunt</span></span>

<span data-ttu-id="f9ddc-155">Grunt viene configurato con un manifesto denominato *Gruntfile.js* che definisce, carica e registra le attività che possono essere eseguite manualmente o a cui è configurate per essere eseguito automaticamente in base sugli eventi in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-155">Grunt is configured using a manifest named *Gruntfile.js* that defines, loads and registers tasks that can be run manually or configured to run automatically based on events in Visual Studio.</span></span>

1.  <span data-ttu-id="f9ddc-156">Fare clic sul progetto e selezionare **Aggiungi > Nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-156">Right-click the project and select **Add > New Item**.</span></span> <span data-ttu-id="f9ddc-157">Selezionare il **file di configurazione Grunt** opzione, lasciare il nome predefinito, *Gruntfile.js*, fare clic su di **Aggiungi** pulsante.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-157">Select the **Grunt Configuration file** option, leave the default name, *Gruntfile.js*, and click the **Add** button.</span></span>

    <span data-ttu-id="f9ddc-158">Il codice iniziale include una definizione di modulo e `grunt.initConfig()` metodo.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-158">The initial code includes a module definition and the `grunt.initConfig()` method.</span></span> <span data-ttu-id="f9ddc-159">Il `initConfig()` viene utilizzato per impostare le opzioni per ogni pacchetto, e il resto del modulo verrà caricato e registrare le attività.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-159">The `initConfig()` is used to set options for each package, and the remainder of the module will load and register tasks.</span></span>
    
    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
      });
    };
    ```

2. <span data-ttu-id="f9ddc-160">All'interno di `initConfig()` metodo, aggiungere le opzioni per il `clean` come illustrato nell'esempio di attività *Gruntfile.js* sotto.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-160">Inside the `initConfig()` method, add options for the `clean` task as shown in the example *Gruntfile.js* below.</span></span> <span data-ttu-id="f9ddc-161">L'attività clean accetta una matrice di stringhe di directory.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-161">The clean task accepts an array of directory strings.</span></span> <span data-ttu-id="f9ddc-162">Questa operazione rimuove i file da wwwroot/lib e la directory temp/intera.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-162">This task removes files from wwwroot/lib and removes the entire /temp directory.</span></span>

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

3. <span data-ttu-id="f9ddc-163">Sotto il metodo initConfig(), aggiungere una chiamata a `grunt.loadNpmTasks()`.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-163">Below the initConfig() method, add a call to `grunt.loadNpmTasks()`.</span></span> <span data-ttu-id="f9ddc-164">In questo modo l'attività eseguibili da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-164">This will make the task runnable from Visual Studio.</span></span>

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

4. <span data-ttu-id="f9ddc-165">Salvare *Gruntfile.js*.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-165">Save *Gruntfile.js*.</span></span> <span data-ttu-id="f9ddc-166">Il file dovrebbe essere simile al seguente schermata riportata di seguito.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-166">The file should look something like the screenshot below.</span></span>

    ![gruntfile iniziale](using-grunt/_static/gruntfile-js-initial.png)

5. <span data-ttu-id="f9ddc-168">Fare doppio clic su *Gruntfile.js* e selezionare **Task Runner Explorer** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-168">Right-click *Gruntfile.js* and select **Task Runner Explorer** from the context menu.</span></span> <span data-ttu-id="f9ddc-169">Verrà aperta la finestra di Esplora esecuzione attività.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-169">The Task Runner Explorer window will open.</span></span>

    ![menu di Task runner explorer](using-grunt/_static/task-runner-explorer-menu.png)

6. <span data-ttu-id="f9ddc-171">Verificare che `clean` visualizzato nell'area **attività** in Task Runner Explorer.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-171">Verify that `clean` shows under **Tasks** in the Task Runner Explorer.</span></span>

    ![elenco delle attività Task runner explorer](using-grunt/_static/task-runner-explorer-tasks.png)

7. <span data-ttu-id="f9ddc-173">L'attività clean e scegliere **eseguire** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-173">Right-click the clean task and select **Run** from the context menu.</span></span> <span data-ttu-id="f9ddc-174">Una finestra di comando consente di visualizzare lo stato di avanzamento dell'attività.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-174">A command window displays progress of the task.</span></span>

    ![Task runner explorer eseguire attività di pulizia dell'](using-grunt/_static/task-runner-explorer-run-clean.png)
    
    > [!NOTE]
    > <span data-ttu-id="f9ddc-176">Non esistono file o directory per eseguire la pulizia ancora.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-176">There are no files or directories to clean yet.</span></span> <span data-ttu-id="f9ddc-177">Se si desidera, è possibile creare manualmente in Esplora soluzioni e quindi eseguire l'attività clean come test.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-177">If you like, you can manually create them in the Solution Explorer and then run the clean task as a test.</span></span>
    
8. <span data-ttu-id="f9ddc-178">Aggiungere una voce per il metodo initConfig(), `concat` utilizzando il codice riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-178">In the initConfig() method, add an entry for `concat` using the code below.</span></span>

    <span data-ttu-id="f9ddc-179">Il `src` matrice proprietà elenca i file per combinare, nell'ordine che devono essere combinati.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-179">The `src` property array lists files to combine, in the order that they should be combined.</span></span> <span data-ttu-id="f9ddc-180">Il `dest` proprietà assegna il percorso del file combinato che viene generato.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-180">The `dest` property assigns the path to the combined file that's produced.</span></span>

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```
    
    > [!NOTE]
    > <span data-ttu-id="f9ddc-181">Il `all` proprietà nel codice precedente è il nome di una destinazione.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-181">The `all` property in the code above is the name of a target.</span></span> <span data-ttu-id="f9ddc-182">Le destinazioni vengono utilizzate in alcune attività Grunt per consentire più ambienti di compilazione.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-182">Targets are used in some Grunt tasks to allow multiple build environments.</span></span> <span data-ttu-id="f9ddc-183">È possibile visualizzare le destinazioni predefinite utilizzando Intellisense o assegnare la propria.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-183">You can view the built-in targets using Intellisense or assign your own.</span></span>
    
9. <span data-ttu-id="f9ddc-184">Aggiungere il `jshint` attività utilizzando il codice riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-184">Add the `jshint` task using the code below.</span></span>

    <span data-ttu-id="f9ddc-185">L'utilità di qualità del codice jshint viene eseguito su tutti i file JavaScript trovato nella directory temporanea.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-185">The jshint code-quality utility is run against every JavaScript file found in the temp directory.</span></span>
    
    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > <span data-ttu-id="f9ddc-186">L'opzione "-W069" è un errore generato da jshint quando JavaScript venga utilizzata la sintassi per l'assegnazione di una proprietà anziché la notazione del punto, ad esempio a racchiudere tra parentesi quadre `Tastes["Sweet"]` anziché `Tastes.Sweet`.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-186">The option "-W069" is an error produced by jshint when JavaScript uses bracket syntax to assign a property instead of dot notation, i.e. `Tastes["Sweet"]` instead of `Tastes.Sweet`.</span></span> <span data-ttu-id="f9ddc-187">L'opzione consente di disattivare l'avviso per consentire il resto del processo di continuare.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-187">The option turns off the warning to allow the rest of the process to continue.</span></span>

10.  <span data-ttu-id="f9ddc-188">Aggiungere il `uglify` attività utilizzando il codice riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-188">Add the `uglify` task using the code below.</span></span>

    <span data-ttu-id="f9ddc-189">Minimizza l'attività di *combined.js* file trovato nella directory temporanea e crea il file dei risultati in wwwroot/lib che seguono la convenzione di denominazione standard  *\<nome file\>. min.js*.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-189">The task minifies the *combined.js* file found in the temp directory and creates the result file in wwwroot/lib following the standard naming convention *\<file name\>.min.js*.</span></span>
    
    ```javascript
    uglify: {
      all: {
        src: ['temp/combined.js'],
        dest: 'wwwroot/lib/combined.min.js'
      }
    },
    ```

11. <span data-ttu-id="f9ddc-190">In grunt.loadNpmTasks() di chiamata che carica grunt-pensionistici pulire, includere la stessa chiamata per jshint, concat e uglify utilizzando il codice riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-190">Under the call grunt.loadNpmTasks() that loads grunt-contrib-clean, include the same call for jshint, concat and uglify using the code below.</span></span>
    
    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

12. <span data-ttu-id="f9ddc-191">Salvare *Gruntfile.js*.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-191">Save *Gruntfile.js*.</span></span> <span data-ttu-id="f9ddc-192">Il file dovrebbe essere simile all'esempio riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-192">The file should look something like the example below.</span></span>

    ![esempio di file grunt completo](using-grunt/_static/gruntfile-js-complete.png)

13. <span data-ttu-id="f9ddc-194">Si noti che l'elenco di attività di Task Runner Explorer include `clean`, `concat`, `jshint` e `uglify` attività.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-194">Notice that the Task Runner Explorer Tasks list includes `clean`, `concat`, `jshint` and `uglify` tasks.</span></span> <span data-ttu-id="f9ddc-195">Eseguire ogni attività nell'ordine e osservare i risultati in Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-195">Run each task in order and observe the results in Solution Explorer.</span></span> <span data-ttu-id="f9ddc-196">Ogni attività devono essere eseguite senza errori.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-196">Each task should run without errors.</span></span>
    
    ![Esplora esecuzione attività eseguire ogni attività.](using-grunt/_static/task-runner-explorer-run-each-task.png)
    
    <span data-ttu-id="f9ddc-198">Crea una nuova attività concat *combined.js* file e lo inserisce nella directory temporanea.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-198">The concat task creates a new *combined.js* file and places it into the temp directory.</span></span> <span data-ttu-id="f9ddc-199">L'attività jshint semplicemente viene eseguito e non produce output.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-199">The jshint task simply runs and doesn’t produce output.</span></span> <span data-ttu-id="f9ddc-200">Crea una nuova attività uglify *combined.min.js* file e la inserisce in wwwroot/lib.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-200">The uglify task creates a new *combined.min.js* file and places it into wwwroot/lib.</span></span> <span data-ttu-id="f9ddc-201">Al termine, la soluzione dovrebbe apparire come schermata riportata di seguito:</span><span class="sxs-lookup"><span data-stu-id="f9ddc-201">On completion, the solution should look something like the screenshot below:</span></span>
    
    ![al termine di queste attività di Esplora soluzioni](using-grunt/_static/solution-explorer-after-all-tasks.png)
    
    > [!NOTE]
    > <span data-ttu-id="f9ddc-203">Per ulteriori informazioni sulle opzioni per ogni pacchetto, visitare [https://www.npmjs.com/](https://www.npmjs.com/) e ricerca il nome del pacchetto nella casella di ricerca nella pagina principale.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-203">For more information on the options for each package, visit [https://www.npmjs.com/](https://www.npmjs.com/) and lookup the package name in the search box on the main page.</span></span> <span data-ttu-id="f9ddc-204">Ad esempio, è possibile cercare il pacchetto grunt-pensionistici pulire per ottenere un collegamento della documentazione che descrive tutti i relativi parametri.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-204">For example, you can look up the grunt-contrib-clean package to get a documentation link that explains all of its parameters.</span></span>

### <a name="all-together-now"></a><span data-ttu-id="f9ddc-205">Ora è possibile</span><span class="sxs-lookup"><span data-stu-id="f9ddc-205">All together now</span></span>

<span data-ttu-id="f9ddc-206">Utilizzare il Grunt `registerTask()` metodo per eseguire una serie di attività in una determinata sequenza.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-206">Use the Grunt `registerTask()` method to run a series of tasks in a particular sequence.</span></span> <span data-ttu-id="f9ddc-207">Ad esempio, per eseguire l'esempio passaggi illustrati in precedenza nell'ordine pulita -> concat -> jshint -> uglify, aggiungere il codice seguente al modulo.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-207">For example, to run the example steps above in the order clean -> concat -> jshint -> uglify, add the code below to the module.</span></span> <span data-ttu-id="f9ddc-208">Il codice deve essere aggiunto allo stesso livello delle chiamate loadNpmTasks(), all'esterno di initConfig.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-208">The code should be added to the same level as the loadNpmTasks() calls, outside initConfig.</span></span>

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

<span data-ttu-id="f9ddc-209">La nuova attività viene visualizzato in Esplora esecuzione attività in attività di Alias.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-209">The new task shows up in Task Runner Explorer under Alias Tasks.</span></span> <span data-ttu-id="f9ddc-210">È possibile fare doppio clic su ed eseguirlo come si farebbe altre attività.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-210">You can right-click and run it just as you would other tasks.</span></span> <span data-ttu-id="f9ddc-211">Il `all` attività verrà eseguita `clean`, `concat`, `jshint` e `uglify`, in ordine.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-211">The `all` task will run `clean`, `concat`, `jshint` and `uglify`, in order.</span></span>

![attività grunt alias](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a><span data-ttu-id="f9ddc-213">Analizzare le modifiche apportate</span><span class="sxs-lookup"><span data-stu-id="f9ddc-213">Watching for changes</span></span>

<span data-ttu-id="f9ddc-214">Oggetto `watch` attività controlla su file e directory.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-214">A `watch` task keeps an eye on files and directories.</span></span> <span data-ttu-id="f9ddc-215">Le espressioni di controllo Attiva attività automaticamente se vengono rilevate modifiche.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-215">The watch triggers tasks automatically if it detects changes.</span></span> <span data-ttu-id="f9ddc-216">Aggiungere il codice seguente per initConfig per controllare le modifiche a \*file con estensione js nella directory TypeScript.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-216">Add the code below to initConfig to watch for changes to \*.js files in the TypeScript directory.</span></span> <span data-ttu-id="f9ddc-217">Se viene modificato un file JavaScript, `watch` eseguirà il `all` attività.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-217">If a JavaScript file is changed, `watch` will run the `all` task.</span></span>

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

<span data-ttu-id="f9ddc-218">Aggiungere una chiamata a `loadNpmTasks()` per mostrare il `watch` attività in Esplora esecuzione attività.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-218">Add a call to `loadNpmTasks()` to show the `watch` task in Task Runner Explorer.</span></span>

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

<span data-ttu-id="f9ddc-219">L'attività di espressione di controllo in Esplora esecuzione attività di mouse e selezionare Esegui dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-219">Right-click the watch task in Task Runner Explorer and select Run from the context menu.</span></span> <span data-ttu-id="f9ddc-220">Nella finestra di comando che mostra l'attività di espressione di controllo in esecuzione verrà visualizzato un "in attesa..."</span><span class="sxs-lookup"><span data-stu-id="f9ddc-220">The command window that shows the watch task running will display a "Waiting…"</span></span> <span data-ttu-id="f9ddc-221">.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-221">message.</span></span> <span data-ttu-id="f9ddc-222">Aprire un file TypeScript, aggiungere uno spazio e quindi salvare il file.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-222">Open one of the TypeScript files, add a space, and then save the file.</span></span> <span data-ttu-id="f9ddc-223">Questo verrà attivare l'attività di espressione di controllo e attivare le altre attività da eseguire in ordine.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-223">This will trigger the watch task and trigger the other tasks to run in order.</span></span> <span data-ttu-id="f9ddc-224">La schermata seguente mostra un esempio di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-224">The screenshot below shows a sample run.</span></span>

![esegue l'output di attività](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a><span data-ttu-id="f9ddc-226">Associazione a eventi di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f9ddc-226">Binding to Visual Studio events</span></span>

<span data-ttu-id="f9ddc-227">A meno che non si desidera avviare manualmente l'attività ogni volta che si lavora in Visual Studio, è possibile associare le attività **prima di compilare**, **dopo la compilazione**, **Pulisci**, e  **Apri progetto** eventi.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-227">Unless you want to manually start your tasks every time you work in Visual Studio, you can bind tasks to **Before Build**, **After Build**, **Clean**, and **Project Open** events.</span></span>

<span data-ttu-id="f9ddc-228">Consente di associare `watch` in modo che venga eseguito ogni volta che apre Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-228">Let’s bind `watch` so that it runs every time Visual Studio opens.</span></span> <span data-ttu-id="f9ddc-229">In Esplora esecuzione attività, l'attività di espressione di controllo e scegliere **associazioni > Apri progetto** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-229">In Task Runner Explorer, right-click the watch task and select **Bindings > Project Open** from the context menu.</span></span>

![associare un'attività per l'apertura del progetto](using-grunt/_static/bindings-project-open.png)

<span data-ttu-id="f9ddc-231">Scaricare e ricaricare il progetto.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-231">Unload and reload the project.</span></span> <span data-ttu-id="f9ddc-232">Quando viene caricato di nuovo il progetto, l'attività di espressione di controllo verrà avviato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-232">When the project loads again, the watch task will start running automatically.</span></span>

## <a name="summary"></a><span data-ttu-id="f9ddc-233">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="f9ddc-233">Summary</span></span>

<span data-ttu-id="f9ddc-234">Grunt è un canale potente attività che può essere utilizzato per automatizzare la maggior parte delle attività di compilazione di client.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-234">Grunt is a powerful task runner that can be used to automate most client-build tasks.</span></span> <span data-ttu-id="f9ddc-235">Grunt si avvale di NPM per recapitare i pacchetti e le caratteristiche degli strumenti di integrazione con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-235">Grunt leverages NPM to deliver its packages, and features tooling integration with Visual Studio.</span></span> <span data-ttu-id="f9ddc-236">Esplora esecuzione attività di Visual Studio rileva le modifiche apportate ai file di configurazione e fornisce un'interfaccia di facile utilizzo per eseguire attività, visualizzare le attività in esecuzione e associa le attività agli eventi di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f9ddc-236">Visual Studio's Task Runner Explorer detects changes to configuration files and provides a convenient interface to run tasks, view running tasks, and bind tasks to Visual Studio events.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f9ddc-237">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="f9ddc-237">Additional resources</span></span>

   * [<span data-ttu-id="f9ddc-238">Uso di Gulp</span><span class="sxs-lookup"><span data-stu-id="f9ddc-238">Using Gulp</span></span>](using-gulp.md)
