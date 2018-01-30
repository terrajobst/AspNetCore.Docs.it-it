---
title: Utilizzo di Grunt in ASP.NET Core
author: rick-anderson
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/using-grunt
ms.openlocfilehash: c23f170b36ac1b9623835337020f2b5ac9514971
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="using-grunt-in-aspnet-core"></a>Utilizzo di Grunt in ASP.NET Core 

Da [Noel riso](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)

Grunt è un'esecuzione attività JavaScript che consente di automatizzare minimizzazione script, la compilazione di TypeScript, gli strumenti "lint" qualità del codice, pre-processori di CSS e praticamente alcun ricorrenti complessa che deve eseguire per supportare lo sviluppo di client. Grunt è completamente supportato in Visual Studio, anche se i modelli di progetto ASP.NET utilizzano Gulp per impostazione predefinita (vedere [utilizzando Gulp](using-gulp.md)).

In questo esempio utilizza un progetto ASP.NET Core vuoto come punto di partenza, per mostrare come automatizzare il processo di compilazione del client da zero.

Nell'esempio di aver pulisce la directory di distribuzione di destinazione, combina i file JavaScript, controlli di qualità del codice, condensa contenuto di un file JavaScript e distribuisce nella radice dell'applicazione web. Si utilizzerà i pacchetti seguenti:

* **grunt**: Grunt l'attività runner il pacchetto.

* **grunt-pensionistici pulire**: un plug-in che rimuove i file o directory.

* **grunt-pensionistici-jshint**: un plug-in che esamina la qualità del codice JavaScript.

* **grunt-pensionistici-concat**: un plug-in che unisce i file in un singolo file.

* **grunt-pensionistici uglify**: un plug-in che minimizza JavaScript per ridurre le dimensioni.

* **grunt-pensionistici-watch**: un plug-in che esegue attività di file.

## <a name="preparing-the-application"></a>Preparazione dell'applicazione

Per iniziare, impostare una nuova applicazione web vuota e aggiungere i file di esempio TypeScript. I file typeScript vengono compilati automaticamente in JavaScript utilizzando le impostazioni di Visual Studio predefinito e saranno il nostro materie prime per elaborare utilizzando Grunt.

1.  In Visual Studio, creare un nuovo `ASP.NET Web Application`.

2.  Nel **nuovo progetto ASP.NET** finestra di dialogo, selezionare ASP.NET Core **vuoto** modello e fare clic sul pulsante OK.

3.  In Esplora soluzioni, esaminare la struttura del progetto. Il `\src` cartella include vuoto `wwwroot` e `Dependencies` nodi.

    ![soluzione web vuoto](using-grunt/_static/grunt-solution-explorer.png)

4.  Aggiungere una nuova cartella denominata `TypeScript` alla directory del progetto.

5.  Prima di aggiungere tutti i file, assicurarsi che Visual Studio è l'opzione ' compilare al salvataggio ' per i file TypeScript selezionati. Passare a **strumenti** > **opzioni** > **Editor di testo** > **Typescript**  >  **Progetto**:

    ![opzioni di impostazione compliation automatica dei file TypeScript](using-grunt/_static/typescript-options.png)

6.  Fare doppio clic su di `TypeScript` directory e selezionare **Aggiungi > Nuovo elemento** dal menu di scelta rapida. Selezionare il **file JavaScript** elemento e denominare il file *Tastes.ts* (si noti il \*estensione TS). Copiare la riga di codice TypeScript seguente nel file (quando si salva, un nuovo *Tastes.js* file verrà visualizzato con l'origine di JavaScript).
    
    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7.  Aggiungere un secondo file per il **TypeScript** directory e denominarla `Food.ts`. Copiare il codice seguente nel file.

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

## <a name="configuring-npm"></a>Configurazione di NPM

Configurare quindi NPM per scaricare grunt e grunt-attività.

1. In Esplora soluzioni, fare clic sul progetto e selezionare **Aggiungi > Nuovo elemento** dal menu di scelta rapida. Selezionare il **file di configurazione NPM** voce, lasciare il nome predefinito, *package. JSON*, fare clic su di **Aggiungi** pulsante.

2. Nel *package. JSON* file, all'interno di `devDependencies` oggetto tra parentesi graffe, immettere "grunt". Selezionare `grunt` elenco Intellisense e premere INVIO. Visual Studio verrà offerta il nome del pacchetto grunt e aggiungere i due punti. A destra dei due punti, selezionare la versione stabile più recente del pacchetto dalla parte superiore dell'elenco di Intellisense (premere `Ctrl-Space` se Intellisense non è presente).

    ![grun Intellisense](using-grunt/_static/devdependencies-grunt.png)
    
    > [!NOTE]
    > Usa NPM [controllo delle versioni semantico](http://semver.org/) per organizzare le dipendenze. Controllo delle versioni semantico, noto anche come SemVer, identifica i pacchetti con lo schema di numerazione <major>.<minor>. <patch>. IntelliSense semplifica il controllo delle versioni semantico mostrando solo alcune opzioni comuni. Il primo elemento nell'elenco di Intellisense (0.4.5 nell'esempio precedente) viene considerato l'ultima versione stabile del pacchetto. Il simbolo di accento circonflesso (^) corrisponda alla versione principale più recente e la tilde (~) corrisponda alla versione secondaria più recente. Vedere il [riferimento parser versione NPM semver](https://www.npmjs.com/package/semver) come guida per l'espressività completa che fornisce SemVer.

3. Aggiungere altre dipendenze di caricare grunt-pensionistici -\* pacchetti per *pulita*, *jshint*, *concat*, *uglify*e *espressioni di controllo* come illustrato nell'esempio riportato di seguito. Le versioni non necessario in base all'esempio.

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

4. Salvare il *package. JSON* file.

I pacchetti per ogni elemento devDependencies eseguirà il download, insieme a tutti i file che richiede di ogni pacchetto. È possibile trovare i file del pacchetto nel `node_modules` directory abilitando il **Mostra tutti i file** pulsante in Esplora soluzioni.

![grunt node_modules](using-grunt/_static/node-modules.png)

> [!NOTE]
> Se si desidera, è possibile ripristinare manualmente le dipendenze in Esplora soluzioni facendo clic su `Dependencies\NPM` e selezionando il **pacchetti ripristinare** opzione di menu.

![ripristino dei pacchetti](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a>Configurazione Grunt

Grunt viene configurato con un manifesto denominato *Gruntfile.js* che definisce, carica e registra le attività che possono essere eseguite manualmente o a cui è configurate per essere eseguito automaticamente in base sugli eventi in Visual Studio.

1.  Fare clic sul progetto e selezionare **Aggiungi > Nuovo elemento**. Selezionare il **file di configurazione Grunt** opzione, lasciare il nome predefinito, *Gruntfile.js*, fare clic su di **Aggiungi** pulsante.

    Il codice iniziale include una definizione di modulo e `grunt.initConfig()` metodo. Il `initConfig()` viene utilizzato per impostare le opzioni per ogni pacchetto, e il resto del modulo verrà caricato e registrare le attività.
    
    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
      });
    };
    ```

2. All'interno di `initConfig()` metodo, aggiungere le opzioni per il `clean` come illustrato nell'esempio di attività *Gruntfile.js* sotto. L'attività clean accetta una matrice di stringhe di directory. Questa operazione rimuove i file da wwwroot/lib e la directory temp/intera.

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

3. Sotto il metodo initConfig(), aggiungere una chiamata a `grunt.loadNpmTasks()`. In questo modo l'attività eseguibili da Visual Studio.

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

4. Salvare *Gruntfile.js*. Il file dovrebbe essere simile al seguente schermata riportata di seguito.

    ![gruntfile iniziale](using-grunt/_static/gruntfile-js-initial.png)

5. Fare doppio clic su *Gruntfile.js* e selezionare **Task Runner Explorer** dal menu di scelta rapida. Verrà aperta la finestra di Esplora esecuzione attività.

    ![menu di Task runner explorer](using-grunt/_static/task-runner-explorer-menu.png)

6. Verificare che `clean` visualizzato nell'area **attività** in Task Runner Explorer.

    ![elenco delle attività Task runner explorer](using-grunt/_static/task-runner-explorer-tasks.png)

7. L'attività clean e scegliere **eseguire** dal menu di scelta rapida. Una finestra di comando consente di visualizzare lo stato di avanzamento dell'attività.

    ![Task runner explorer eseguire attività di pulizia dell'](using-grunt/_static/task-runner-explorer-run-clean.png)
    
    > [!NOTE]
    > Non esistono file o directory per eseguire la pulizia ancora. Se si desidera, è possibile creare manualmente in Esplora soluzioni e quindi eseguire l'attività clean come test.
    
8. Aggiungere una voce per il metodo initConfig(), `concat` utilizzando il codice riportato di seguito.

    Il `src` matrice proprietà elenca i file per combinare, nell'ordine che devono essere combinati. Il `dest` proprietà assegna il percorso del file combinato che viene generato.

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```
    
    > [!NOTE]
    > Il `all` proprietà nel codice precedente è il nome di una destinazione. Le destinazioni vengono utilizzate in alcune attività Grunt per consentire più ambienti di compilazione. È possibile visualizzare le destinazioni predefinite utilizzando Intellisense o assegnare la propria.
    
9. Aggiungere il `jshint` attività utilizzando il codice riportato di seguito.

    L'utilità di qualità del codice jshint viene eseguito su tutti i file JavaScript trovato nella directory temporanea.
    
    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > L'opzione "-W069" è un errore generato da jshint quando JavaScript venga utilizzata la sintassi per l'assegnazione di una proprietà anziché la notazione del punto, ad esempio a racchiudere tra parentesi quadre `Tastes["Sweet"]` anziché `Tastes.Sweet`. L'opzione consente di disattivare l'avviso per consentire il resto del processo di continuare.

10.  Aggiungere il `uglify` attività utilizzando il codice riportato di seguito.

    Minimizza l'attività di *combined.js* file trovato nella directory temporanea e crea il file dei risultati in wwwroot/lib che seguono la convenzione di denominazione standard  *\<nome file\>. min.js*.
    
    ```javascript
    uglify: {
      all: {
        src: ['temp/combined.js'],
        dest: 'wwwroot/lib/combined.min.js'
      }
    },
    ```

11. In grunt.loadNpmTasks() di chiamata che carica grunt-pensionistici pulire, includere la stessa chiamata per jshint, concat e uglify utilizzando il codice riportato di seguito.
    
    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

12. Salvare *Gruntfile.js*. Il file dovrebbe essere simile all'esempio riportato di seguito.

    ![esempio di file grunt completo](using-grunt/_static/gruntfile-js-complete.png)

13. Si noti che l'elenco di attività di Task Runner Explorer include `clean`, `concat`, `jshint` e `uglify` attività. Eseguire ogni attività nell'ordine e osservare i risultati in Esplora soluzioni. Ogni attività devono essere eseguite senza errori.
    
    ![Esplora esecuzione attività eseguire ogni attività.](using-grunt/_static/task-runner-explorer-run-each-task.png)
    
    Crea una nuova attività concat *combined.js* file e lo inserisce nella directory temporanea. L'attività jshint semplicemente viene eseguito e non produce output. Crea una nuova attività uglify *combined.min.js* file e la inserisce in wwwroot/lib. Al termine, la soluzione dovrebbe apparire come schermata riportata di seguito:
    
    ![al termine di queste attività di Esplora soluzioni](using-grunt/_static/solution-explorer-after-all-tasks.png)
    
    > [!NOTE]
    > Per ulteriori informazioni sulle opzioni per ogni pacchetto, visitare [https://www.npmjs.com/](https://www.npmjs.com/) e ricerca il nome del pacchetto nella casella di ricerca nella pagina principale. Ad esempio, è possibile cercare il pacchetto grunt-pensionistici pulire per ottenere un collegamento della documentazione che descrive tutti i relativi parametri.

### <a name="all-together-now"></a>Ora è possibile

Utilizzare il Grunt `registerTask()` metodo per eseguire una serie di attività in una determinata sequenza. Ad esempio, per eseguire l'esempio passaggi illustrati in precedenza nell'ordine pulita -> concat -> jshint -> uglify, aggiungere il codice seguente al modulo. Il codice deve essere aggiunto allo stesso livello delle chiamate loadNpmTasks(), all'esterno di initConfig.

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

La nuova attività viene visualizzato in Esplora esecuzione attività in attività di Alias. È possibile fare doppio clic su ed eseguirlo come si farebbe altre attività. Il `all` attività verrà eseguita `clean`, `concat`, `jshint` e `uglify`, in ordine.

![attività grunt alias](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a>Analizzare le modifiche apportate

Oggetto `watch` attività controlla su file e directory. Le espressioni di controllo Attiva attività automaticamente se vengono rilevate modifiche. Aggiungere il codice seguente per initConfig per controllare le modifiche a \*file con estensione js nella directory TypeScript. Se viene modificato un file JavaScript, `watch` eseguirà il `all` attività.

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

Aggiungere una chiamata a `loadNpmTasks()` per mostrare il `watch` attività in Esplora esecuzione attività.

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

L'attività di espressione di controllo in Esplora esecuzione attività di mouse e selezionare Esegui dal menu di scelta rapida. Nella finestra di comando che mostra l'attività di espressione di controllo in esecuzione verrà visualizzato un "in attesa..." . Aprire un file TypeScript, aggiungere uno spazio e quindi salvare il file. Questo verrà attivare l'attività di espressione di controllo e attivare le altre attività da eseguire in ordine. La schermata seguente mostra un esempio di esecuzione.

![esegue l'output di attività](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a>Associazione a eventi di Visual Studio

A meno che non si desidera avviare manualmente l'attività ogni volta che si lavora in Visual Studio, è possibile associare le attività **prima di compilare**, **dopo la compilazione**, **Pulisci**, e  **Apri progetto** eventi.

Consente di associare `watch` in modo che venga eseguito ogni volta che apre Visual Studio. In Esplora esecuzione attività, l'attività di espressione di controllo e scegliere **associazioni > Apri progetto** dal menu di scelta rapida.

![associare un'attività per l'apertura del progetto](using-grunt/_static/bindings-project-open.png)

Scaricare e ricaricare il progetto. Quando viene caricato di nuovo il progetto, l'attività di espressione di controllo verrà avviato automaticamente.

## <a name="summary"></a>Riepilogo

Grunt è un canale potente attività che può essere utilizzato per automatizzare la maggior parte delle attività di compilazione di client. Grunt si avvale di NPM per recapitare i pacchetti e le caratteristiche degli strumenti di integrazione con Visual Studio. Esplora esecuzione attività di Visual Studio rileva le modifiche apportate ai file di configurazione e fornisce un'interfaccia di facile utilizzo per eseguire attività, visualizzare le attività in esecuzione e associa le attività agli eventi di Visual Studio.

## <a name="additional-resources"></a>Risorse aggiuntive

   * [Uso di Gulp](using-gulp.md)
