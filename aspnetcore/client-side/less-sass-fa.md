---
title: Minore, Sass e tipo di carattere straordinario in ASP.NET Core
author: ardalis
description: Informazioni su come utilizzare minore, Sass e tipo di carattere straordinario nelle applicazioni ASP.NET Core.
ms.author: tdykstra
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/less-sass-fa
ms.openlocfilehash: 764b11bbd301c0116488265d32f7d46dfc5bce27
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-styling-applications-with-less-sass-and-font-awesome-in-aspnet-core"></a><span data-ttu-id="d0ccc-103">Introduzione alle applicazioni stile con meno, Sass e carattere straordinario in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d0ccc-103">Introduction to styling applications with Less, Sass, and Font Awesome in ASP.NET Core</span></span>

<span data-ttu-id="d0ccc-104">Da [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="d0ccc-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="d0ccc-105">Se si desidera applicare uno stile e l'esperienza complessiva, gli utenti delle applicazioni web hanno aspettative sempre più elevate.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-105">Users of web applications have increasingly high expectations when it comes to style and overall experience.</span></span> <span data-ttu-id="d0ccc-106">Applicazioni web moderne spesso sfruttano strumenti avanzati e Framework per definire e gestire l'aspetto in modo coerente.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-106">Modern web applications frequently leverage rich tools and frameworks for defining and managing their look and feel in a consistent manner.</span></span> <span data-ttu-id="d0ccc-107">Framework come [Bootstrap](http://getbootstrap.com/) possono passare in modo che definisce un set comune di stili e le opzioni di layout per i siti web.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-107">Frameworks like [Bootstrap](http://getbootstrap.com/) can go a long way toward defining a common set of styles and layout options for web sites.</span></span> <span data-ttu-id="d0ccc-108">Tuttavia, la maggior parte dei siti non semplice anche trarre vantaggio dalla possibilità di definire in modo efficace e Gestisci stili e i file di foglio di stile CSS, nonché l'accesso semplice a icone non di immagine che consentono di rendere più intuitiva interfaccia del sito.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-108">However, most non-trivial sites also benefit from being able to effectively define and maintain styles and cascading style sheet (CSS) files, as well as having easy access to non-image icons that help make the site's interface more intuitive.</span></span> <span data-ttu-id="d0ccc-109">Corretta linguaggi e strumenti che supportano [meno](http://lesscss.org/) e [Sass](http://sass-lang.com/), e le librerie come [carattere straordinario](http://fontawesome.io/), sono disponibili in.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-109">That's where languages and tools that support [Less](http://lesscss.org/) and [Sass](http://sass-lang.com/), and libraries like [Font Awesome](http://fontawesome.io/), come in.</span></span>

## <a name="css-preprocessor-languages"></a><span data-ttu-id="d0ccc-110">Lingue per il preprocessore CSS</span><span class="sxs-lookup"><span data-stu-id="d0ccc-110">CSS preprocessor languages</span></span>

<span data-ttu-id="d0ccc-111">Lingue che vengono compilate in altri linguaggi, allo scopo di migliorare l'esperienza di utilizzo del linguaggio sottostante, sono detti i preprocessori.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-111">Languages that are compiled into other languages, in order to improve the experience of working with the underlying language, are referred to as preprocessors.</span></span> <span data-ttu-id="d0ccc-112">Esistono due i preprocessori diffusi per CSS: minore e Sass.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-112">There are two popular preprocessors for CSS: Less and Sass.</span></span>  <span data-ttu-id="d0ccc-113">Questi preprocessori aggiungere funzionalità a CSS, ad esempio il supporto per le variabili e regole nidificate, migliorare la gestibilità dei fogli di stile grandi e complesse.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-113">These preprocessors add features to CSS, such as support for variables and nested rules, which improve the maintainability of large, complex stylesheets.</span></span> <span data-ttu-id="d0ccc-114">CSS come linguaggio è molto semplice, mancanza di supporto anche per un'operazione semplice come variabili e ciò tende a rendere i file CSS ricorrenti e troppo grandi.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-114">CSS as a language is very basic, lacking support even for something as simple as variables, and this tends to make CSS files repetitive and bloated.</span></span> <span data-ttu-id="d0ccc-115">Aggiunta di funzionalità del linguaggio reale programmazione tramite i preprocessori consentono di ridurre la duplicazione e offrire una migliore organizzazione delle regole di stile.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-115">Adding real programming language features via preprocessors can help reduce duplication and provide better organization of styling rules.</span></span> <span data-ttu-id="d0ccc-116">Visual Studio fornisce supporto incorporato per entrambi minore e Sass, nonché le estensioni che consentono di migliorare ulteriormente l'esperienza di sviluppo quando si lavora con questi linguaggi.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-116">Visual Studio provides built-in support for both Less and Sass, as well as extensions that can further improve the development experience when working with these languages.</span></span>

<span data-ttu-id="d0ccc-117">Un rapido esempio di come i preprocessori possono migliorare la leggibilità e facilità di gestione delle informazioni di stile, considerare il CSS:</span><span class="sxs-lookup"><span data-stu-id="d0ccc-117">As a quick example of how preprocessors can improve readability and maintainability of style information, consider this CSS:</span></span>

```css
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    color: black;
    font-weight: bold;
    font-size: 14px;
    font-family: Helvetica, Arial, sans-serif;
}
```

<span data-ttu-id="d0ccc-118">Utilizza minore, questo può essere riscritto per eliminare tutti i duplicati, utilizzando un *mixin* (tale nome perché consente di "mix in" proprietà da una classe o un set di regole in un altro):</span><span class="sxs-lookup"><span data-stu-id="d0ccc-118">Using Less, this can be rewritten to eliminate all of the duplication, using a *mixin* (so named because it allows you to "mix in" properties from one class or rule-set into another):</span></span>

```less
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    .header;
    font-size: 14px;
}
```

## <a name="less"></a><span data-ttu-id="d0ccc-119">minore</span><span class="sxs-lookup"><span data-stu-id="d0ccc-119">Less</span></span>

<span data-ttu-id="d0ccc-120">L'esecuzione del preprocessore meno CSS utilizzando Node.js.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-120">The Less CSS preprocessor runs using Node.js.</span></span> <span data-ttu-id="d0ccc-121">Per installare minore, usare Gestione pacchetti di nodi (npm) da un prompt dei comandi (-g significa "globale"):</span><span class="sxs-lookup"><span data-stu-id="d0ccc-121">To install Less, use Node Package Manager (npm) from a command prompt (-g means "global"):</span></span>

```console
npm install -g less
```

<span data-ttu-id="d0ccc-122">Se si utilizza Visual Studio, è possibile iniziare con meno aggiungendo meno uno o più file al progetto, e quindi configurando Gulp (o Grunt) per l'elaborazione in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-122">If you're using Visual Studio, you can get started with Less by adding one or more Less files to your project, and then configuring Gulp (or Grunt) to process them at compile-time.</span></span> <span data-ttu-id="d0ccc-123">Aggiungere un *stili* cartella al progetto, quindi aggiungere un nuovo meno file denominato *main.less* in questa cartella.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-123">Add a *Styles* folder to your project, and then add a new Less file named *main.less* to this folder.</span></span>

![Aggiungere file less](less-sass-fa/_static/add-less-file.png)

<span data-ttu-id="d0ccc-125">Una volta aggiunta, la struttura di cartelle dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="d0ccc-125">Once added, your folder structure should look something like this:</span></span>

![Struttura di cartelle](less-sass-fa/_static/folder-structure.png)

<span data-ttu-id="d0ccc-127">Ora è possibile aggiungere alcuni lo stile di base per il file, che verrà distribuito nella cartella wwwroot da Gulp e compilato in CSS.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-127">Now you can add some basic styling to the file, which will be compiled into CSS and deployed to the wwwroot folder by Gulp.</span></span>

<span data-ttu-id="d0ccc-128">Modificare *main.less* per includere il contenuto seguente, che consente di creare una tavolozza dei colori semplice da un singolo colore di base.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-128">Modify *main.less* to include the following content, which creates a simple color palette from a single base color.</span></span>

```less
@base: #663333;
@background: spin(@base, 180);
@lighter: lighten(spin(@base, 5), 10%);
@lighter2: lighten(spin(@base, 10), 20%);
@darker: darken(spin(@base, -5), 10%);
@darker2: darken(spin(@base, -10), 20%);

body {
    background-color:@background;
}
.baseColor  {color:@base}
.bgLight    {color:@lighter}
.bgLight2   {color:@lighter2}
.bgDark     {color:@darker}
.bgDark2    {color:@darker2}
```

<span data-ttu-id="d0ccc-129">`@base`e l'altro @-prefixed gli elementi sono variabili.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-129">`@base` and the other @-prefixed items are variables.</span></span> <span data-ttu-id="d0ccc-130">Ognuno di essi rappresenta un colore.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-130">Each of them represents a color.</span></span> <span data-ttu-id="d0ccc-131">Ad eccezione di `@base`, si impostano utilizzando funzioni di colore: schiarire, viene resa più scura e ruotare.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-131">Except for `@base`, they're set using color functions: lighten, darken, and spin.</span></span> <span data-ttu-id="d0ccc-132">Rendere più chiari e più scuri eseguire praticamente alle aspettative; selezione consente di regolare la tonalità di un colore di un numero di gradi (circa il selettore di colore).</span><span class="sxs-lookup"><span data-stu-id="d0ccc-132">Lighten and darken do pretty much what you would expect; spin adjusts the hue of a color by a number of degrees (around the color wheel).</span></span> <span data-ttu-id="d0ccc-133">Il minore processore è abbastanza per ignorare le variabili che non sono utilizzate per illustrare il funzionano di queste variabili, è necessario utilizzarle in un punto.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-133">The Less processor is smart enough to ignore variables that aren't used, so to demonstrate how these variables work, we need to use them somewhere.</span></span> <span data-ttu-id="d0ccc-134">Le classi `.baseColor`, e così via consentiranno di dimostrare i valori calcolati di ciascuna delle variabili nel file CSS che viene generato.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-134">The classes `.baseColor`, etc. will demonstrate the calculated values of each of the variables in the CSS file that's produced.</span></span>

### <a name="getting-started"></a><span data-ttu-id="d0ccc-135">Per iniziare</span><span class="sxs-lookup"><span data-stu-id="d0ccc-135">Getting started</span></span>

<span data-ttu-id="d0ccc-136">Creare un **File di configurazione npm** (*package. JSON*) nella cartella del progetto e modificarlo per fare riferimento a `gulp` e `gulp-less`:</span><span class="sxs-lookup"><span data-stu-id="d0ccc-136">Create an **npm Configuration File** (*package.json*) in your project folder and edit it to reference `gulp` and `gulp-less`:</span></span>

```json
{
  "version": "1.0.0",
  "name": "asp.net",
  "private": true,
  "devDependencies": {
    "gulp": "3.9.1",
    "gulp-less": "3.3.0"
  }
}
```

<span data-ttu-id="d0ccc-137">Installare le dipendenze in un prompt dei comandi nella cartella del progetto o in Visual Studio **Esplora** (**dipendenze > npm > ripristino dei pacchetti**).</span><span class="sxs-lookup"><span data-stu-id="d0ccc-137">Install the dependencies either at a command prompt in your project folder, or in Visual Studio **Solution Explorer** (**Dependencies > npm > Restore packages**).</span></span>

```console
npm install
```

![Ripristino dei pacchetti Visual Studio](less-sass-fa/_static/restore-packages.png)

<span data-ttu-id="d0ccc-139">Nella cartella del progetto, creare un **Gulp File di configurazione** (*gulpfile.js*) per definire il processo automatico.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-139">In the project folder, create a **Gulp Configuration File** (*gulpfile.js*) to define the automated process.</span></span>  <span data-ttu-id="d0ccc-140">Aggiungere una variabile nella parte superiore del file per rappresentare minore e un'attività da eseguire minore:</span><span class="sxs-lookup"><span data-stu-id="d0ccc-140">Add a variable at the top of the file to represent Less, and a task to run Less:</span></span>

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less");

gulp.task("less", function () {
  return gulp.src('Styles/main.less')
    .pipe(less())
    .pipe(gulp.dest('wwwroot/css'));
});
```

<span data-ttu-id="d0ccc-141">Aprire il **Esplora esecuzione attività** (**Vista > altre finestre > Esplora esecuzione attività**).</span><span class="sxs-lookup"><span data-stu-id="d0ccc-141">Open the **Task Runner Explorer** (**View > Other Windows > Task Runner Explorer**).</span></span> <span data-ttu-id="d0ccc-142">Tra le attività, si noterà una nuova attività denominata `less`.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-142">Among the tasks, you should see a new task named `less`.</span></span> <span data-ttu-id="d0ccc-143">Potrebbe essere necessario aggiornare la finestra.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-143">You might have to refresh the window.</span></span>

<span data-ttu-id="d0ccc-144">Eseguire il `less` attività ed è visualizzato un output simile a quello mostrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="d0ccc-144">Run the `less` task, and you see output similar to what is shown here:</span></span>

![esecuzione di minore attività](less-sass-fa/_static/less-task-runner.png)

<span data-ttu-id="d0ccc-146">Il *wwwroot/css* cartella contiene ora un nuovo file, *Main. CSS*:</span><span class="sxs-lookup"><span data-stu-id="d0ccc-146">The *wwwroot/css* folder now contains a new file, *main.css*:</span></span>

![css principale creato](less-sass-fa/_static/main-css-created.png)

<span data-ttu-id="d0ccc-148">Aprire *Main. CSS* e viene visualizzato qualcosa di simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="d0ccc-148">Open *main.css* and you see something like the following:</span></span>

```css
body {
    background-color: #336666;
}
.baseColor {
    color: #663333;
}
.bgLight {
    color: #884a44;
}
.bgLight2 {
    color: #aa6355;
}
.bgDark {
    color: #442225;
}
.bgDark2 {
    color: #221114;
}
```

<span data-ttu-id="d0ccc-149">Aggiungere una semplice pagina HTML per il *wwwroot* cartella e riferimento *Main. CSS* per visualizzare la tavolozza dei colori in azione.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-149">Add a simple HTML page to the *wwwroot* folder, and reference *main.css* to see the color palette in action.</span></span>

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <link href="css/main.css" rel="stylesheet" />
    <title></title>
</head>
<body>
    <div>
        <div class="baseColor">BaseColor</div>
        <div class="bgLight">Light</div>
        <div class="bgLight2">Light2</div>
        <div class="bgDark">Dark</div>
        <div class="bgDark2">Dark2</div>
    </div>
</body>
</html>
```

<span data-ttu-id="d0ccc-150">È possibile vedere 180 gradi di selezione in `@base` utilizzato per produrre `@background` ha restituito il selettore di colore contrapposte colore di `@base`:</span><span class="sxs-lookup"><span data-stu-id="d0ccc-150">You can see that the 180 degree spin on `@base` used to produce `@background` resulted in the color wheel opposing color of `@base`:</span></span>

![esempio meno di test](less-sass-fa/_static/less-test-screenshot.png)

<span data-ttu-id="d0ccc-152">Minore fornisce anche supporto per regole nidificate, nonché le query nidificate media.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-152">Less also provides support for nested rules, as well as nested media queries.</span></span> <span data-ttu-id="d0ccc-153">Ad esempio, le gerarchie annidate che definisce come le regole CSS dettagliate possono comportare menu come queste:</span><span class="sxs-lookup"><span data-stu-id="d0ccc-153">For example, defining nested hierarchies like menus can result in verbose CSS rules like these:</span></span>

```css
nav {
    height: 40px;
    width: 100%;
}
nav li {
    height: 38px;
    width: 100px;
}
nav li a:link {
    color: #000;
    text-decoration: none;
}
nav li a:visited {
    text-decoration: none;
    color: #CC3333;
}
nav li a:hover {
    text-decoration: underline;
    font-weight: bold;
}
nav li a:active {
    text-decoration: underline;
}
```

<span data-ttu-id="d0ccc-154">In teoria tutte le regole di stile correlati verranno posizionate all'interno del file CSS, ma in pratica non c'è niente applicare questa regola, ad eccezione di convenzione e forse commenti del blocco.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-154">Ideally all of the related style rules will be placed together within the CSS file, but in practice there's nothing enforcing this rule except convention and perhaps block comments.</span></span>

<span data-ttu-id="d0ccc-155">Definizione di queste stesse regole di minore utilizzo è simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="d0ccc-155">Defining these same rules using Less looks like this:</span></span>

```less
nav {
    height: 40px;
    width: 100%;
    li {
        height: 38px;
        width: 100px;
        a {
            color: #000;
            &:link { text-decoration:none}
            &:visited { color: #CC3333; text-decoration:none}
            &:hover { text-decoration:underline; font-weight:bold}
            &:active {text-decoration:underline}
        }
    }
}
```

<span data-ttu-id="d0ccc-156">Si noti che in questo caso, tutti gli elementi subordinati di `nav` sono contenuti all'interno dell'ambito.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-156">Note that in this case, all of the subordinate elements of `nav` are contained within its scope.</span></span> <span data-ttu-id="d0ccc-157">Non è più il ripetersi degli elementi padre (`nav`, `li`, `a`), e il conteggio delle righe totali è stato eliminato anche (anche se alcune delle che non è un risultato di inserimento di valori nelle stesse linee nel secondo esempio).</span><span class="sxs-lookup"><span data-stu-id="d0ccc-157">There's no longer any repetition of parent elements (`nav`, `li`, `a`), and the total line count has dropped as well (though some of that's a result of putting values on the same lines in the second example).</span></span> <span data-ttu-id="d0ccc-158">Può essere molto utile, organizationally, per visualizzare tutte le regole per un determinato elemento dell'interfaccia utente all'interno di un ambito in modo esplicito limitato, in questo caso disattivare dal resto del file da parentesi graffe.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-158">It can be very helpful, organizationally, to see all of the rules for a given UI element within an explicitly bounded scope, in this case set off from the rest of the file by curly braces.</span></span>

<span data-ttu-id="d0ccc-159">Il `&` sintassi è una funzionalità meno selettore con & rappresenta l'oggetto padre selettore corrente.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-159">The `&` syntax is a Less selector feature, with & representing the current selector parent.</span></span> <span data-ttu-id="d0ccc-160">In questo caso, all'interno di un {...}</span><span class="sxs-lookup"><span data-stu-id="d0ccc-160">So, within the a {...}</span></span> <span data-ttu-id="d0ccc-161">blocco, `&` rappresenta un `a` tag e pertanto `&:link` equivale a `a:link`.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-161">block, `&` represents an `a` tag, and thus `&:link` is equivalent to `a:link`.</span></span>

<span data-ttu-id="d0ccc-162">Supporti le query molto utili nella creazione di progettazioni reattive, possono inoltre contribuire attivamente ripetizioni e complessità in CSS.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-162">Media queries, extremely useful in creating responsive designs, can also contribute heavily to repetition and complexity in CSS.</span></span> <span data-ttu-id="d0ccc-163">Minore consente supporti le query all'interno di classi, in modo che la definizione di classe per intero non deve essere ripetuto all'interno di diversi principale `@media` elementi.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-163">Less allows media queries to be nested within classes, so that the entire class definition doesn't need to be repeated within different top-level `@media` elements.</span></span> <span data-ttu-id="d0ccc-164">Ad esempio, ecco CSS per un menu reattivo:</span><span class="sxs-lookup"><span data-stu-id="d0ccc-164">For example, here is CSS for a responsive menu:</span></span>

```css
.navigation {
    margin-top: 30%;
    width: 100%;
}
@media screen and (min-width: 40em) {
    .navigation {
        margin: 0;
    }
}
@media screen and (min-width: 62em) {
    .navigation {
        width: 960px;
        margin: 0;
    }
}
```

<span data-ttu-id="d0ccc-165">Ciò può essere meglio definito in meno di:</span><span class="sxs-lookup"><span data-stu-id="d0ccc-165">This can be better defined in Less as:</span></span>

```less
.navigation {
    margin-top: 30%;
    width: 100%;
    @media screen and (min-width: 40em) {
        margin: 0;
    }
    @media screen and (min-width: 62em) {
        width: 960px;
        margin: 0;
    }
}
```

<span data-ttu-id="d0ccc-166">Un'altra funzionalità minore che abbiamo già visto è il supporto di operazioni matematiche, consentendo di attributi di stile deve essere costruita da variabili predefinite.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-166">Another feature of Less that we have already seen is its support for mathematical operations, allowing style attributes to be constructed from pre-defined variables.</span></span> <span data-ttu-id="d0ccc-167">In questo modo l'aggiornamento di stili correlati molto più semplici, poiché la variabile di base può essere modificata e tutti i valori dipendenti cambiano automaticamente.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-167">This makes updating related styles much easier, since the base variable can be modified and all dependent values change automatically.</span></span>

<span data-ttu-id="d0ccc-168">File CSS, soprattutto per i siti di grandi dimensioni (e in particolare se vengono utilizzate query media), tendono a ottenere notevole nel tempo, rendendo difficile da gestire loro utilizzo.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-168">CSS files, especially for large sites (and especially if media queries are being used), tend to get quite large over time, making working with them unwieldy.</span></span> <span data-ttu-id="d0ccc-169">File less possono essere definiti separatamente, quindi uniti utilizzando `@import` direttive.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-169">Less files can be defined separately, then pulled together using `@import` directives.</span></span> <span data-ttu-id="d0ccc-170">Minore può anche essere utilizzato per importare singoli file CSS, nonché, se lo si desidera.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-170">Less can also be used to import individual CSS files, as well, if desired.</span></span>

<span data-ttu-id="d0ccc-171">*"Mixins"* può accettare parametri e minore supporta una logica condizionale sotto forma di protezioni mixin, che forniscono una modalità dichiarativa per definire quando effettive determinate "mixins".</span><span class="sxs-lookup"><span data-stu-id="d0ccc-171">*Mixins* can accept parameters, and Less supports conditional logic in the form of mixin guards, which provide a declarative way to define when certain mixins take effect.</span></span> <span data-ttu-id="d0ccc-172">Un utilizzo comune per le protezioni mixin per modificare i colori in modo chiaro o scuro il colore di origine.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-172">A common use for mixin guards is to adjust colors based on how light or dark the source color is.</span></span> <span data-ttu-id="d0ccc-173">Dato un parametro mixin che accetta un parametro per il colore, una guardia mixin consente di modificare il mixin basata su tale colore:</span><span class="sxs-lookup"><span data-stu-id="d0ccc-173">Given a mixin that accepts a parameter for color, a mixin guard can be used to modify the mixin based on that color:</span></span>

```less
.box (@color) when (lightness(@color) >= 50%) {
    background-color: #000;
}
.box (@color) when (lightness(@color) < 50%) {
    background-color: #FFF;
}
.box (@color) {
    color: @color;
}

.feature {
    .box (@base);
}
```

<span data-ttu-id="d0ccc-174">Dato il corrente `@base` valore `#663333`, meno lo script genererà il codice CSS seguente:</span><span class="sxs-lookup"><span data-stu-id="d0ccc-174">Given our current `@base` value of `#663333`, this Less script will produce the following CSS:</span></span>

```css
.feature {
    background-color: #FFF;
    color: #663333;
}
```

<span data-ttu-id="d0ccc-175">Minore fornisce una serie di funzionalità aggiuntive, ma questo deve fornire un'idea della potenza di questo linguaggio di pre-elaborazione.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-175">Less provides a number of additional features, but this should give you some idea of the power of this preprocessing language.</span></span>

## <a name="sass"></a><span data-ttu-id="d0ccc-176">Sass</span><span class="sxs-lookup"><span data-stu-id="d0ccc-176">Sass</span></span>

<span data-ttu-id="d0ccc-177">Sass è simile a un valore inferiore, che fornisce supporto per molte delle stesse funzionalità, ma con una sintassi leggermente diversa.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-177">Sass is similar to Less, providing support for many of the same features, but with slightly different syntax.</span></span> <span data-ttu-id="d0ccc-178">Viene compilata utilizzando Ruby anziché JavaScript e pertanto dispone di requisiti di installazione diverso.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-178">It's built using Ruby, rather than JavaScript, and so has different setup requirements.</span></span> <span data-ttu-id="d0ccc-179">La lingua Sass originale non utilizza le parentesi graffe o punti e virgola, ma invece definita ambito utilizzando lo spazio vuoto e il rientro.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-179">The original Sass language didn't use curly braces or semicolons, but instead defined scope using white space and indentation.</span></span> <span data-ttu-id="d0ccc-180">Una nuova sintassi introdotta nella versione 3 di Sass, **SCSS** "Sassy CSS (").</span><span class="sxs-lookup"><span data-stu-id="d0ccc-180">In version 3 of Sass, a new syntax was introduced, **SCSS** ("Sassy CSS").</span></span> <span data-ttu-id="d0ccc-181">SCSS è simile a CSS, in quanto Ignora spazi vuoti e i livelli di rientro e si utilizza un punto e virgola e parentesi graffe.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-181">SCSS is similar to CSS in that it ignores indentation levels and whitespace, and instead uses semicolons and curly braces.</span></span>

<span data-ttu-id="d0ccc-182">Per installare Sass, in genere si sarebbe installare Ruby (pre-installato su Mac) e quindi eseguire:</span><span class="sxs-lookup"><span data-stu-id="d0ccc-182">To install Sass, typically you would first install Ruby (pre-installed on Mac), and then run:</span></span>

```console
gem install sass
```

<span data-ttu-id="d0ccc-183">Tuttavia, se si esegue Visual Studio, è possibile iniziare a usare Sass in gran parte esattamente come si farebbe con minore.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-183">However, if you're running Visual Studio, you can get started with Sass in much the same way as you would with Less.</span></span> <span data-ttu-id="d0ccc-184">Aprire *package. JSON* e aggiungere il pacchetto "gulp-sass" `devDependencies`:</span><span class="sxs-lookup"><span data-stu-id="d0ccc-184">Open *package.json* and add the "gulp-sass" package to `devDependencies`:</span></span>

```json
"devDependencies": {
  "gulp": "3.9.1",
  "gulp-less": "3.3.0",
  "gulp-sass": "3.1.0"
}
```

<span data-ttu-id="d0ccc-185">Successivamente, modificare *gulpfile.js* per aggiungere una variabile sass e un'attività per compilare i file Sass e inserire i risultati nella cartella wwwroot:</span><span class="sxs-lookup"><span data-stu-id="d0ccc-185">Next, modify *gulpfile.js* to add a sass variable and a task to compile your Sass files and place the results in the wwwroot folder:</span></span>

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less"),
  sass = require("gulp-sass");

// other content removed

gulp.task("sass", function () {
  return gulp.src('Styles/main2.scss')
    .pipe(sass())
    .pipe(gulp.dest('wwwroot/css'));
});
```

<span data-ttu-id="d0ccc-186">È ora possibile aggiungere il file Sass *main2.scss* per il *stili* cartella nella radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="d0ccc-186">Now you can add the Sass file *main2.scss* to the *Styles* folder in the root of the project:</span></span>

![aggiungere file scss](less-sass-fa/_static/add-scss-file.png)

<span data-ttu-id="d0ccc-188">Aprire *main2.scss* e aggiungere le seguenti:</span><span class="sxs-lookup"><span data-stu-id="d0ccc-188">Open *main2.scss* and add the following:</span></span>

```sass
$base: #CC0000;
body {
    background-color: $base;
}
```

<span data-ttu-id="d0ccc-189">Salvare tutti i file.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-189">Save all of your files.</span></span> <span data-ttu-id="d0ccc-190">Quando si aggiorna ora **Task Runner Explorer**, vedrai un `sass` attività.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-190">Now when you refresh **Task Runner Explorer**, you see a `sass` task.</span></span> <span data-ttu-id="d0ccc-191">Eseguirlo ed esaminare il */wwwroot/css* cartella.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-191">Run it, and look in the */wwwroot/css* folder.</span></span> <span data-ttu-id="d0ccc-192">È ora disponibile un *main2.css* file, con questo tipo di contenuto:</span><span class="sxs-lookup"><span data-stu-id="d0ccc-192">There's now a *main2.css* file, with these contents:</span></span>

```css
body {
    background-color: #CC0000;
}
```

<span data-ttu-id="d0ccc-193">Sass supporta l'annidamento nello stesso stato che minore non, fornendo vantaggi simili.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-193">Sass supports nesting in much the same was that Less does, providing similar benefits.</span></span> <span data-ttu-id="d0ccc-194">File può essere suddiviso dalla funzione e incluso utilizzando il `@import` direttiva:</span><span class="sxs-lookup"><span data-stu-id="d0ccc-194">Files can be split up by function and included using the `@import` directive:</span></span>

```sass
@import 'anotherfile';
```

<span data-ttu-id="d0ccc-195">Sass supporta "mixins", nonché l'utilizzo di `@mixin` (parola chiave) per definirli e `@include` per includerli, come in questo esempio dalla [sass lang.com](http://sass-lang.com):</span><span class="sxs-lookup"><span data-stu-id="d0ccc-195">Sass supports mixins as well, using the `@mixin` keyword to define them and `@include` to include them, as in this example from [sass-lang.com](http://sass-lang.com):</span></span>

```sass
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box { @include border-radius(10px); }
```

<span data-ttu-id="d0ccc-196">Oltre a "mixins", Sass supporta inoltre il concetto di ereditarietà, consentendo una classe estendere un altro.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-196">In addition to mixins, Sass also supports the concept of inheritance, allowing one class to extend another.</span></span> <span data-ttu-id="d0ccc-197">È concettualmente simile a un parametro mixin, ma risulta meno codice CSS.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-197">It's conceptually similar to a mixin, but results in less CSS code.</span></span> <span data-ttu-id="d0ccc-198">Viene utilizzata la `@extend` (parola chiave).</span><span class="sxs-lookup"><span data-stu-id="d0ccc-198">It's accomplished using the `@extend` keyword.</span></span> <span data-ttu-id="d0ccc-199">Per provare "mixins", aggiungere il comando seguente per il *main2.scss* file:</span><span class="sxs-lookup"><span data-stu-id="d0ccc-199">To try out mixins, add the following to your *main2.scss* file:</span></span>

```sass
@mixin alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @include alert;
    border-color: green;
}

.error {
    @include alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

<span data-ttu-id="d0ccc-200">Esaminare l'output in *main2.css* dopo l'esecuzione di `sass` attività in **Task Runner Explorer**:</span><span class="sxs-lookup"><span data-stu-id="d0ccc-200">Examine the output in *main2.css* after running the `sass` task in **Task Runner Explorer**:</span></span>

```css
.success {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    border-color: green;
 }

.error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    color: red;
    border-color: red;
    font-weight: bold;
}
```

<span data-ttu-id="d0ccc-201">Si noti che tutte le proprietà comuni di mixin di avviso vengono ripetute in ogni classe.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-201">Notice that all of the common properties of the alert mixin are repeated in each class.</span></span> <span data-ttu-id="d0ccc-202">Il mixin ha un ottimo lavoro di aiutare a evitare la duplicazione in fase di sviluppo, ma viene comunque creata CSS con molta la duplicazione, risultante di dimensioni superiori al file CSS necessari - un potenziale problema di prestazioni.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-202">The mixin did a good job of helping eliminate duplication at development time, but it's still creating CSS with a lot of duplication in it, resulting in larger than necessary CSS files - a potential performance issue.</span></span>

<span data-ttu-id="d0ccc-203">Sostituire il mixin avviso con un `.alert` classe e modificare `@include` a `@extend` (ricordando di estendere `.alert`, non `alert`):</span><span class="sxs-lookup"><span data-stu-id="d0ccc-203">Now replace the alert mixin with a `.alert` class, and change `@include` to `@extend` (remembering to extend `.alert`, not `alert`):</span></span>

```sass
.alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @extend .alert;
    border-color: green;
}

.error {
    @extend .alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

<span data-ttu-id="d0ccc-204">Eseguire ancora una volta Sass ed esaminare il codice risultante CSS:</span><span class="sxs-lookup"><span data-stu-id="d0ccc-204">Run Sass once more, and examine the resulting CSS:</span></span>

```css
.alert, .success, .error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    border-color: green;
}

.error {
    color: red;
    border-color: red;
    font-weight: bold;
}
```

<span data-ttu-id="d0ccc-205">Ora le proprietà vengono definite solo il numero di volte in base alle necessità, e prestazioni migliori in cui viene generato CSS.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-205">Now the properties are defined only as many times as needed, and better CSS is generated.</span></span>

<span data-ttu-id="d0ccc-206">Sass include anche le funzioni e operazioni di logica condizionale, simile a un valore inferiore.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-206">Sass also includes functions and conditional logic operations, similar to Less.</span></span> <span data-ttu-id="d0ccc-207">In effetti, le funzionalità di due linguaggi sono molto simili.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-207">In fact, the two languages' capabilities are very similar.</span></span>

## <a name="less-or-sass"></a><span data-ttu-id="d0ccc-208">Minore o Sass?</span><span class="sxs-lookup"><span data-stu-id="d0ccc-208">Less or Sass?</span></span>

<span data-ttu-id="d0ccc-209">Non è ancora un consenso che indica se è in genere preferibile utilizzare minore o Sass (o anche se preferisce il Sass originale o la sintassi più recente di SCSS in Sass).</span><span class="sxs-lookup"><span data-stu-id="d0ccc-209">There's still no consensus as to whether it's generally better to use Less or Sass (or even whether to prefer the original Sass or the newer SCSS syntax within Sass).</span></span> <span data-ttu-id="d0ccc-210">Probabilmente la decisione più importante consiste nel **utilizzare uno di questi strumenti**, in contrapposizione alla appena-codifica manuale dei file CSS.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-210">Probably the most important decision is to **use one of these tools**, as opposed to just hand-coding your CSS files.</span></span> <span data-ttu-id="d0ccc-211">Dopo aver apportato che delle decisioni, entrambi minore e Sass costituiscono un'ottima scelta.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-211">Once you've made that decision, both Less and Sass are good choices.</span></span>

## <a name="font-awesome"></a><span data-ttu-id="d0ccc-212">Tipo di carattere straordinario</span><span class="sxs-lookup"><span data-stu-id="d0ccc-212">Font Awesome</span></span>

<span data-ttu-id="d0ccc-213">Oltre ai preprocessori CSS, un'altra risorsa ottima per applicazioni web moderne lo stile è straordinario del carattere.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-213">In addition to CSS preprocessors, another great resource for styling modern web applications is Font Awesome.</span></span> <span data-ttu-id="d0ccc-214">Tipo di carattere amici sono un toolkit che fornisce le icone del vettore scalabile oltre 500 che possono essere usate liberamente in applicazioni web.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-214">Font Awesome is a toolkit that provides over 500 scalable vector icons that can be freely used in your web applications.</span></span> <span data-ttu-id="d0ccc-215">È stato originariamente progettato per funzionare con Bootstrap, ma non dispone di alcuna dipendenza su tale framework o su tutte le librerie JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-215">It was originally designed to work with Bootstrap, but it has no dependency on that framework or on any JavaScript libraries.</span></span>

<span data-ttu-id="d0ccc-216">Il modo più semplice per iniziare con tipo di carattere straordinario consiste nell'aggiungere un riferimento a esso, utilizzando il percorso di rete (CDN) di recapito di contenuto pubblico:</span><span class="sxs-lookup"><span data-stu-id="d0ccc-216">The easiest way to get started with Font Awesome is to add a reference to it, using its public content delivery network (CDN) location:</span></span>

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
```

<span data-ttu-id="d0ccc-217">È possibile anche aggiungerlo al progetto di Visual Studio aggiungendolo al "dipendenze" in *bower. JSON*:</span><span class="sxs-lookup"><span data-stu-id="d0ccc-217">You can also add it to your Visual Studio project by adding it to the "dependencies" in *bower.json*:</span></span>

```json
{
  "name": "ASP.NET",
  "private": true,
  "dependencies": {
    "bootstrap": "3.0.0",
    "jquery": "1.10.2",
    "jquery-validation": "1.11.1",
    "jquery-validation-unobtrusive": "3.2.2",
    "hammer.js": "2.0.4",
    "bootstrap-touch-carousel": "0.8.0",
    "Font-Awesome": "4.3.0"
  }
}
```

<span data-ttu-id="d0ccc-218">Dopo aver creato un riferimento a carattere straordinario in una pagina, è possibile aggiungere le icone all'applicazione tramite l'applicazione di tipo di carattere straordinario classi, in genere è precedute da "fa-", per gli elementi HTML inline (ad esempio `<span>` o `<i>`).</span><span class="sxs-lookup"><span data-stu-id="d0ccc-218">Once you have a reference to Font Awesome on a page, you can add icons to your application by applying Font Awesome classes, typically prefixed with "fa-", to your inline HTML elements (such as `<span>` or `<i>`).</span></span>  <span data-ttu-id="d0ccc-219">Ad esempio, è possibile aggiungere le icone per elenchi semplici e menu usando codice simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="d0ccc-219">For example, you can add icons to simple lists and menus using code like this:</span></span>

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title></title>
    <link href="lib/font-awesome/css/font-awesome.css" rel="stylesheet" />
</head>
<body>
    <ul class="fa-ul">
        <li><i class="fa fa-li fa-home"></i> Home</li>
        <li><i class="fa fa-li fa-cog"></i> Settings</li>
    </ul>
</body>
</html>
```

<span data-ttu-id="d0ccc-220">Ciò produce il seguente nel browser - si noti l'icona accanto a ogni elemento:</span><span class="sxs-lookup"><span data-stu-id="d0ccc-220">This produces the following in the browser - note the icon beside each item:</span></span>

![elenco icone](less-sass-fa/_static/list-icons-screenshot.png)

<span data-ttu-id="d0ccc-222">È possibile visualizzare un elenco completo delle icone disponibili:</span><span class="sxs-lookup"><span data-stu-id="d0ccc-222">You can view a complete list of the available icons here:</span></span>

<span data-ttu-id="d0ccc-223">http://fontawesome.io/icons/</span><span class="sxs-lookup"><span data-stu-id="d0ccc-223">http://fontawesome.io/icons/</span></span>

## <a name="summary"></a><span data-ttu-id="d0ccc-224">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="d0ccc-224">Summary</span></span>

<span data-ttu-id="d0ccc-225">Applicazioni web moderne richiedono sempre più reattive, fluido progettazioni di pulito, intuitiva e facile da usare da un'ampia gamma di dispositivi.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-225">Modern web applications increasingly demand responsive, fluid designs that are clean, intuitive, and easy to use from a variety of devices.</span></span> <span data-ttu-id="d0ccc-226">Gestire la complessità di fogli di stile CSS, necessarie per raggiungere questi obiettivi è più adatto utilizzano meno di un tipo per il preprocessore o Sass.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-226">Managing the complexity of the CSS stylesheets required to achieve these goals is best done using a preprocessor like Less or Sass.</span></span> <span data-ttu-id="d0ccc-227">Inoltre, Toolkit come tipo di carattere straordinario possa fornire rapidamente le icone note ai menu di navigazione testuale e i pulsanti, l'utente complessiva di migliorare l'esperienza dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d0ccc-227">In addition, toolkits like Font Awesome quickly provide well-known icons to textual navigation menus and buttons, improving the overall user experience of your application.</span></span>
