---
title: Usare Grunt in ASP.NET Core
author: rick-anderson
description: Usare Grunt in ASP.NET Core
ms.author: riande
ms.date: 05/14/2019
uid: client-side/using-grunt
ms.openlocfilehash: 4d9b6cf6f9a0007e9722bc054f0d9a7608f1473b
ms.sourcegitcommit: 3ee6ee0051c3d2c8d47a58cb17eef1a84a4c46a0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/14/2019
ms.locfileid: "65620992"
---
# <a name="use-grunt-in-aspnet-core"></a>Usare Grunt in ASP.NET Core

Da [riso Noel](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)

Grunt è uno strumento di esecuzione attività JavaScript che automatizza la minimizzazione dello script, la compilazione TypeScript, gli strumenti "lint" qualità del codice, pre-processori CSS e praticamente qualsiasi lavoro ingrato ripetitiva che deve eseguire per supportare lo sviluppo di client. Grunt è completamente supportato in Visual Studio.

Questo esempio Usa un progetto ASP.NET Core vuoto come punto di partenza, in cui viene illustrato come automatizzare il processo di compilazione di client da zero.

L'esempio completato pulisce la directory di distribuzione di destinazione, combina i file JavaScript, controlli di qualità del codice, condensa contenuto del file JavaScript e distribuisce nella radice dell'applicazione web. Verranno usati i pacchetti seguenti:

* **grunt**: Il pacchetto di strumento di esecuzione attività Grunt.

* **grunt-contrib-clean**: Un plug-in che consente di rimuovere file o directory.

* **grunt-contrib-jshint**: Un plug-in che esamina la qualità del codice JavaScript.

* **grunt-contrib-concat**: Un plug-in che consente di unire i file in un singolo file.

* **grunt-contrib-uglify**: Un plug-in che minimizza JavaScript per ridurre le dimensioni.

* **grunt-contrib-watch**: Un plug-in che verifica la presenza di attività di file.

## <a name="preparing-the-application"></a>Preparazione dell'applicazione

Per iniziare, configurare una nuova applicazione web vuota e aggiungere i file di esempio di TypeScript. File TypeScript vengono compilati automaticamente in JavaScript utilizzando le impostazioni di Visual Studio predefinite e saranno nostro materie prime per elaborare l'uso di Grunt.

1. In Visual Studio, creare un nuovo `ASP.NET Web Application`.

2. Nel **nuovo progetto ASP.NET** finestra di dialogo selezionare ASP.NET Core **vuota** modello e fare clic sul pulsante OK.

3. In Esplora soluzioni, esaminare la struttura del progetto. Il `\src` cartella include vuota `wwwroot` e `Dependencies` nodi.

    ![soluzione web vuoto](using-grunt/_static/grunt-solution-explorer.png)

4. Aggiungere una nuova cartella denominata `TypeScript` alla directory del progetto.

5. Prima di aggiungere tutti i file, assicurarsi che Visual Studio include l'opzione ' Compila al salvataggio ' per i file TypeScript selezionati. Passare a **degli strumenti** > **opzioni** > **Editor di testo** > **Typescript**  >  **Progetto**:

    ![opzioni di impostazione di compilazione automatico dei file TypeScript](using-grunt/_static/typescript-options.png)

6. Fare doppio clic il `TypeScript` directory e selezionare **Aggiungi > Nuovo elemento** dal menu di scelta rapida. Selezionare il **file JavaScript** elemento e denominare il file *Tastes.ts* (nota di \*estensione TS). Copiare la riga del codice TypeScript seguente nel file (quando si salva, una nuova *Tastes.js* file verrà visualizzato con l'origine di JavaScript).

    ```typescript
    enum Tastes { Sweet, Sour, Salty, Bitter }
    ```

7. Aggiungere un secondo file per il **TypeScript** directory e denominarlo `Food.ts`. Copiare il codice seguente nel file.

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

## <a name="configuring-npm"></a>Configurazione NPM

A questo punto, configurare NPM per scaricare grunt e attività di grunt.

1. In Esplora soluzioni, fare clic sul progetto e selezionare **Aggiungi > Nuovo elemento** dal menu di scelta rapida. Selezionare il **file di configurazione NPM** voce, lasciare il nome predefinito, *package. JSON*, fare clic sul **Aggiungi** pulsante.

2. Nel *package. JSON* nel file, all'interno di `devDependencies` oggetto parentesi graffe, immettere "grunt". Selezionare `grunt` Intellisense elenco e premere INVIO. Visual Studio verrà racchiudere tra virgolette il nome del pacchetto grunt e aggiungere i due punti. A destra dei due punti, selezionare la versione stabile più recente del pacchetto dalla parte superiore dell'elenco di Intellisense (premere `Ctrl-Space` se Intellisense non viene visualizzata).

    ![grunt Intellisense](using-grunt/_static/devdependencies-grunt.png)

    > [!NOTE]
    > NPM Usa [versionamento semantico](http://semver.org/) per organizzare le dipendenze. Versionamento semantico, noto anche come SemVer, identifica i pacchetti con lo schema di numerazione \<principale >.\< secondaria >. \<patch >. IntelliSense semplifica versionamento semantico mostrando solo alcune opzioni comuni. Il primo elemento nell'elenco di Intellisense (0.4.5 nell'esempio precedente) viene considerato la versione stabile più recente del pacchetto. Il simbolo di accento circonflesso (^) corrisponde alla versione principale più recente e la tilde (~) corrisponde alla versione secondaria più recente. Vedere le [riferimenti ai parser versione di NPM semver](https://www.npmjs.com/package/semver) come guida per l'espressività completa che fornisce SemVer.

3. Aggiungere altre dipendenze per caricare grunt-contrib -\* pacchetti relativi *pulita*, *jshint*, *concat*, *uglify*e *watch* come illustrato nell'esempio seguente. Le versioni non necessario in base all'esempio.

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

I pacchetti per ogni `devDependencies` elemento scaricherà, insieme a eventuali file necessari per ogni pacchetto. È possibile trovare i file del pacchetto nel *node_modules* directory abilitando le **Mostra tutti i file** sul pulsante **Esplora soluzioni**.

![node_modules grunt](using-grunt/_static/node-modules.png)

> [!NOTE]
> Se si desidera, è possibile ripristinare manualmente le dipendenze nel **Esplora soluzioni** facendo clic su `Dependencies\NPM` e selezionando la **Ripristina pacchetti** l'opzione di menu.

![il ripristino dei pacchetti](using-grunt/_static/restore-packages.png)

## <a name="configuring-grunt"></a>Configurazione di Grunt

Grunt viene configurato mediante un manifesto denominato *gruntfile. js* che definisce, carica e registra le attività che possono essere eseguite manualmente o configurate per essere eseguito automaticamente in base sugli eventi in Visual Studio.

1. Fare clic sul progetto e selezionare **Add** > **nuovo elemento**. Selezionare il **JavaScript File** modello di elemento, modificare il nome in *gruntfile. js*, fare clic sui **Add** pulsante.

1. Aggiungere il codice seguente a *gruntfile. js*. Il `initConfig` funzione imposta le opzioni per ogni pacchetto e il resto del modulo Carica e registra le attività.

   ```javascript
   module.exports = function (grunt) {
     grunt.initConfig({
     });
   };
   ```

1. All'interno di `initConfig` funzione, aggiungere le opzioni per il `clean` come illustrato nell'esempio di attività *gruntfile. js* sotto. Il `clean` attività accetta una matrice di stringhe di directory. Questa attività consente di rimuovere file dalla *wwwroot/lib* e rimuove l'intera */temp* directory.

    ```javascript
    module.exports = function (grunt) {
      grunt.initConfig({
        clean: ["wwwroot/lib/*", "temp/"],
      });
    };
    ```

1. Di seguito il `initConfig` funzione, aggiungere una chiamata a `grunt.loadNpmTasks`. In questo modo le attività eseguibili da Visual Studio.

    ```javascript
    grunt.loadNpmTasks("grunt-contrib-clean");
    ```

1. Salvare *gruntfile. js*. Il file dovrebbe essere simile alla schermata seguente.

    ![gruntfile iniziale](using-grunt/_static/gruntfile-js-initial.png)

1. Fare doppio clic su *gruntfile. js* e selezionare **Task Runner Explorer** dal menu di scelta rapida. Il **Task Runner Explorer** aprirà la finestra.

    ![menu di scelta Task runner explorer](using-grunt/_static/task-runner-explorer-menu.png)

1. Verificare che `clean` visualizzato nell'area **attività** nel **Task Runner Explorer**.

    ![elenco delle attività Task runner explorer](using-grunt/_static/task-runner-explorer-tasks.png)

1. L'attività clean e scegliere **eseguire** dal menu di scelta rapida. Una finestra di comando consente di visualizzare lo stato di avanzamento dell'attività.

    ![Task runner explorer eseguire attività di pulizia dell'](using-grunt/_static/task-runner-explorer-run-clean.png)

    > [!NOTE]
    > Non esistono file o directory da pulire ancora. Se si desidera, è possibile crearle manualmente in Esplora soluzioni e quindi eseguire l'attività clean come test.

1. Nel `initConfig` funzione, aggiungere una voce per `concat` usando il codice seguente.

    Il `src` matrice proprietà elenca i file per combinare, nell'ordine in cui devono essere combinati. Il `dest` proprietà assegna il percorso del file combinato che viene generato.

    ```javascript
    concat: {
      all: {
        src: ['TypeScript/Tastes.js', 'TypeScript/Food.js'],
        dest: 'temp/combined.js'
      }
    },
    ```

    > [!NOTE]
    > Il `all` proprietà nel codice precedente è il nome di una destinazione. Le destinazioni vengono utilizzate in alcune attività di Grunt per consentire più ambienti di compilazione. È possibile visualizzare le destinazioni predefinite usando IntelliSense o assegnare il proprio.

1. Aggiungere il `jshint` attività usando il codice seguente.

    Il jshint `code-quality` utilità viene eseguita su tutti i file JavaScript disponibili nel *temp* directory.

    ```javascript
    jshint: {
      files: ['temp/*.js'],
      options: {
        '-W069': false,
      }
    },
    ```

    > [!NOTE]
    > L'opzione "-W069" è un errore generato da jshint quando JavaScript venga utilizzata la sintassi per assegnare una proprietà anziché la notazione del punto, vale a dire a racchiudere tra parentesi quadre `Tastes["Sweet"]` invece di `Tastes.Sweet`. L'opzione Disattiva l'avviso per consentire il resto del processo per continuare.

1. Aggiungere il `uglify` attività usando il codice seguente.

    L'attività minimizza la *combined.js* file trovato nella directory temp e crea il file dei risultati in wwwroot/lib seguono la convenzione di denominazione standard  *\<nome file\>. min.js*.

    ```javascript
    uglify: {
     all: {
       src: ['temp/combined.js'],
       dest: 'wwwroot/lib/combined.min.js'
     }
    },
    ```

1. Sotto la chiamata a `grunt.loadNpmTasks` che carica `grunt-contrib-clean`, includere la stessa chiamata per jshint, concat e uglify usando il codice seguente.

    ```javascript
    grunt.loadNpmTasks('grunt-contrib-jshint');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-uglify');
    ```

1. Salvare *gruntfile. js*. Il file dovrebbe essere simile all'esempio riportato di seguito.

    ![esempio di file di grunt completo](using-grunt/_static/gruntfile-js-complete.png)

1. Si noti che il **Task Runner Explorer** elenco attività comprende `clean`, `concat`, `jshint` e `uglify` attività. Eseguire ogni attività nell'ordine e osservare i risultati nella **Esplora soluzioni**. Ogni attività dovrebbero funzionare senza errori.

    ![Esplora esecuzione attività eseguire ogni attività](using-grunt/_static/task-runner-explorer-run-each-task.png)

    Crea una nuova attività concat *combined.js* file e lo inserisce nella directory temporanea. Il `jshint` attività semplicemente in esecuzione e non genera output. Il `uglify` crea una nuova attività *combined.min.js* del file e lo posiziona in *wwwroot/lib*. Al termine, la soluzione dovrebbe essere qualcosa di simile allo screenshot seguente:

    ![Dopo che tutte le attività di Esplora soluzioni](using-grunt/_static/solution-explorer-after-all-tasks.png)

    > [!NOTE]
    > Per altre informazioni sulle opzioni per ogni pacchetto, visitare [ https://www.npmjs.com/ ](https://www.npmjs.com/) e ricerca il nome del pacchetto nella casella di ricerca nella pagina principale. Ad esempio, è possibile cercare il pacchetto grunt-contrib-pulizia per ottenere un collegamento alla documentazione che illustra tutti i relativi parametri.

### <a name="all-together-now"></a>Ora è possibile

Uso di Grunt `registerTask()` metodo per eseguire una serie di attività in una determinata sequenza. Ad esempio, per eseguire l'esempio -> passaggi precedenti nell'ordine pulita concat -> jshint -> uglify, aggiungere il codice seguente al modulo. Il codice deve essere aggiunto allo stesso livello delle chiamate loadNpmTasks(), all'esterno di initConfig.

```javascript
grunt.registerTask("all", ['clean', 'concat', 'jshint', 'uglify']);
```

La nuova attività viene visualizzato in Esplora esecuzione attività nell'area attività Alias. È possibile fare doppio clic su ed eseguirlo esattamente come si farebbe altre attività. Il `all` verrà eseguita l'operazione `clean`, `concat`, `jshint` e `uglify`, nell'ordine.

![attività di grunt alias](using-grunt/_static/alias-tasks.png)

## <a name="watching-for-changes"></a>Controllo per la ricerca di modifiche

Oggetto `watch` attività tiene sotto controllo i file e directory. Se rileva le modifiche, le espressioni di controllo attiva automaticamente le attività. Aggiungere il codice seguente a initConfig controllare le modifiche a \*file con estensione js nella directory di TypeScript. Se viene modificato un file JavaScript, `watch` eseguirà il `all` attività.

```javascript
watch: {
  files: ["TypeScript/*.js"],
  tasks: ["all"]
}
```

Aggiungere una chiamata a `loadNpmTasks()` per mostrare il `watch` attività in Task Runner Explorer.

```javascript
grunt.loadNpmTasks('grunt-contrib-watch');
```

L'attività di espressione di controllo in Task Runner Explorer e scegliere Esegui dal menu di scelta rapida. Nella finestra di comando che mostra l'attività di espressione di controllo in esecuzione verrà visualizzato un "in attesa..." Messaggio. Aprire uno dei file TypeScript, aggiungere uno spazio e quindi salvare il file. Ciò attiva l'attività di espressione di controllo e attivare le altre attività da eseguire in ordine. La schermata seguente mostra un esempio di esecuzione.

![output delle attività in esecuzione](using-grunt/_static/watch-running.png)

## <a name="binding-to-visual-studio-events"></a>Associazione agli eventi di Visual Studio

A meno che non si desidera avviare manualmente l'attività ogni volta che si lavora in Visual Studio, è possibile associare delle attività **prima di compilare**, **dopo la compilazione**, **Pulisci**, e  **Progetto Open** gli eventi.

È possibile associare `watch` in modo che venga eseguito ogni volta che viene aperto Visual Studio. In Task Runner Explorer, l'attività di espressione di controllo e scegliere **associazioni > Apri progetto** dal menu di scelta rapida.

![associare un'attività per l'apertura del progetto](using-grunt/_static/bindings-project-open.png)

Scaricare e ricaricare il progetto. Quando il progetto viene caricato anche in questo caso, l'attività di espressione di controllo verrà avviati automaticamente.

## <a name="summary"></a>Riepilogo

Grunt è uno strumento di esecuzione di attività potente che può essere utilizzato per automatizzare la maggior parte delle attività di compilazione di client. Monitoraggio prestazioni rete per offrire le funzionalità di integrazione con Visual Studio degli strumenti e i pacchetti, si avvale di grunt. Task Runner Explorer di Visual Studio rileva le modifiche apportate ai file di configurazione e fornisce una comoda interfaccia per eseguire le attività, visualizzare le attività in esecuzione e per associare attività agli eventi di Visual Studio.
