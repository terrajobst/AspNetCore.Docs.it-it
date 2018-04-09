---
title: Minore, Sass e tipo di carattere straordinario in ASP.NET Core
author: ardalis
description: Informazioni su come utilizzare minore, Sass e tipo di carattere straordinario nelle applicazioni ASP.NET Core.
manager: wpickett
ms.author: tdykstra
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/less-sass-fa
ms.openlocfilehash: 3bb1c9006f8633485a420b52b5fa9b91b1875cc5
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
---
# <a name="less-sass-and-font-awesome-in-aspnet-core"></a>Minore, Sass e tipo di carattere straordinario in ASP.NET Core

Di [Steve Smith](https://ardalis.com/)

Se si desidera applicare uno stile e l'esperienza complessiva, gli utenti delle applicazioni web hanno aspettative sempre più elevate. Applicazioni web moderne spesso sfruttano strumenti avanzati e Framework per definire e gestire l'aspetto in modo coerente. Framework come [Bootstrap](http://getbootstrap.com/) possono passare in modo che definisce un set comune di stili e le opzioni di layout per i siti web. Tuttavia, la maggior parte dei siti non semplice anche trarre vantaggio dalla possibilità di definire in modo efficace e Gestisci stili e i file di foglio di stile CSS, nonché l'accesso semplice a icone non di immagine che consentono di rendere più intuitiva interfaccia del sito. Corretta linguaggi e strumenti che supportano [meno](http://lesscss.org/) e [Sass](http://sass-lang.com/), e le librerie come [carattere straordinario](http://fontawesome.io/), sono disponibili in.

## <a name="css-preprocessor-languages"></a>Lingue per il preprocessore CSS

Lingue che vengono compilate in altri linguaggi, allo scopo di migliorare l'esperienza di utilizzo del linguaggio sottostante, sono detti i preprocessori. Esistono due i preprocessori diffusi per CSS: minore e Sass.  Questi preprocessori aggiungere funzionalità a CSS, ad esempio il supporto per le variabili e regole nidificate, migliorare la gestibilità dei fogli di stile grandi e complesse. CSS come linguaggio è molto semplice, mancanza di supporto anche per un'operazione semplice come variabili e ciò tende a rendere i file CSS ricorrenti e troppo grandi. Aggiunta di funzionalità del linguaggio reale programmazione tramite i preprocessori consentono di ridurre la duplicazione e offrire una migliore organizzazione delle regole di stile. Visual Studio fornisce supporto incorporato per entrambi minore e Sass, nonché le estensioni che consentono di migliorare ulteriormente l'esperienza di sviluppo quando si lavora con questi linguaggi.

Un rapido esempio di come i preprocessori possono migliorare la leggibilità e facilità di gestione delle informazioni di stile, considerare il CSS:

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

Utilizza minore, questo può essere riscritto per eliminare tutti i duplicati, utilizzando un *mixin* (tale nome perché consente di "mix in" proprietà da una classe o un set di regole in un altro):

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

## <a name="less"></a>minore

L'esecuzione del preprocessore meno CSS utilizzando Node.js. Per installare minore, usare Gestione pacchetti di nodi (npm) da un prompt dei comandi (-g significa "globale"):

```console
npm install -g less
```

Se si utilizza Visual Studio, è possibile iniziare con meno aggiungendo meno uno o più file al progetto, e quindi configurando Gulp (o Grunt) per l'elaborazione in fase di compilazione. Aggiungere un *stili* cartella al progetto, quindi aggiungere un nuovo meno file denominato *main.less* in questa cartella.

![Aggiungere file less](less-sass-fa/_static/add-less-file.png)

Una volta aggiunta, la struttura di cartelle dovrebbe essere simile al seguente:

![Struttura di cartelle](less-sass-fa/_static/folder-structure.png)

Ora è possibile aggiungere alcuni lo stile di base per il file, che verrà distribuito nella cartella wwwroot da Gulp e compilato in CSS.

Modificare *main.less* per includere il contenuto seguente, che consente di creare una tavolozza dei colori semplice da un singolo colore di base.

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

`@base` e l'altro @-prefixed gli elementi sono variabili. Ognuno di essi rappresenta un colore. Ad eccezione di `@base`, si impostano utilizzando funzioni di colore: schiarire, viene resa più scura e ruotare. Rendere più chiari e più scuri eseguire praticamente alle aspettative; selezione consente di regolare la tonalità di un colore di un numero di gradi (circa il selettore di colore). Il minore processore è abbastanza per ignorare le variabili che non sono utilizzate per illustrare il funzionano di queste variabili, è necessario utilizzarle in un punto. Le classi `.baseColor`, e così via consentiranno di dimostrare i valori calcolati di ciascuna delle variabili nel file CSS che viene generato.

### <a name="get-started"></a>Introduzione

Creare un **File di configurazione npm** (*package. JSON*) nella cartella del progetto e modificarlo per fare riferimento a `gulp` e `gulp-less`:

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

Installare le dipendenze in un prompt dei comandi nella cartella del progetto o in Visual Studio **Esplora** (**dipendenze > npm > ripristino dei pacchetti**).

```console
npm install
```

![Ripristino dei pacchetti Visual Studio](less-sass-fa/_static/restore-packages.png)

Nella cartella del progetto, creare un **Gulp File di configurazione** (*gulpfile.js*) per definire il processo automatico.  Aggiungere una variabile nella parte superiore del file per rappresentare minore e un'attività da eseguire minore:

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

Aprire il **Esplora esecuzione attività** (**Vista > altre finestre > Esplora esecuzione attività**). Tra le attività, si noterà una nuova attività denominata `less`. Potrebbe essere necessario aggiornare la finestra.

Eseguire il `less` attività ed è visualizzato un output simile a quello mostrato di seguito:

![esecuzione di minore attività](less-sass-fa/_static/less-task-runner.png)

Il *wwwroot/css* cartella contiene ora un nuovo file, *Main. CSS*:

![css principale creato](less-sass-fa/_static/main-css-created.png)

Aprire *Main. CSS* e viene visualizzato qualcosa di simile al seguente:

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

Aggiungere una semplice pagina HTML per il *wwwroot* cartella e riferimento *Main. CSS* per visualizzare la tavolozza dei colori in azione.

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

È possibile vedere 180 gradi di selezione in `@base` utilizzato per produrre `@background` ha restituito il selettore di colore contrapposte colore di `@base`:

![esempio meno di test](less-sass-fa/_static/less-test-screenshot.png)

Minore fornisce anche supporto per regole nidificate, nonché le query nidificate media. Ad esempio, le gerarchie annidate che definisce come le regole CSS dettagliate possono comportare menu come queste:

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

In teoria tutte le regole di stile correlati verranno posizionate all'interno del file CSS, ma in pratica non c'è niente applicare questa regola, ad eccezione di convenzione e forse commenti del blocco.

Definizione di queste stesse regole di minore utilizzo è simile al seguente:

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

Si noti che in questo caso, tutti gli elementi subordinati di `nav` sono contenuti all'interno dell'ambito. Non è più il ripetersi degli elementi padre (`nav`, `li`, `a`), e il conteggio delle righe totali è stato eliminato anche (anche se alcune delle che non è un risultato di inserimento di valori nelle stesse linee nel secondo esempio). Può essere molto utile, organizationally, per visualizzare tutte le regole per un determinato elemento dell'interfaccia utente all'interno di un ambito in modo esplicito limitato, in questo caso disattivare dal resto del file da parentesi graffe.

Il `&` sintassi è una funzionalità meno selettore con & rappresenta l'oggetto padre selettore corrente. In questo caso, all'interno di un {...} blocco, `&` rappresenta un `a` tag e pertanto `&:link` equivale a `a:link`.

Supporti le query molto utili nella creazione di progettazioni reattive, possono inoltre contribuire attivamente ripetizioni e complessità in CSS. Minore consente supporti le query all'interno di classi, in modo che la definizione di classe per intero non deve essere ripetuto all'interno di diversi principale `@media` elementi. Ad esempio, ecco CSS per un menu reattivo:

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

Ciò può essere meglio definito in meno di:

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

Un'altra funzionalità minore che abbiamo già visto è il supporto di operazioni matematiche, consentendo di attributi di stile deve essere costruita da variabili predefinite. In questo modo l'aggiornamento di stili correlati molto più semplici, poiché la variabile di base può essere modificata e tutti i valori dipendenti cambiano automaticamente.

File CSS, soprattutto per i siti di grandi dimensioni (e in particolare se vengono utilizzate query media), tendono a ottenere notevole nel tempo, rendendo difficile da gestire loro utilizzo. File less possono essere definiti separatamente, quindi uniti utilizzando `@import` direttive. Minore può anche essere utilizzato per importare singoli file CSS, nonché, se lo si desidera.

*"Mixins"* può accettare parametri e minore supporta una logica condizionale sotto forma di protezioni mixin, che forniscono una modalità dichiarativa per definire quando effettive determinate "mixins". Un utilizzo comune per le protezioni mixin per modificare i colori in modo chiaro o scuro il colore di origine. Dato un parametro mixin che accetta un parametro per il colore, una guardia mixin consente di modificare il mixin basata su tale colore:

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

Dato il corrente `@base` valore `#663333`, meno lo script genererà il codice CSS seguente:

```css
.feature {
    background-color: #FFF;
    color: #663333;
}
```

Minore fornisce una serie di funzionalità aggiuntive, ma questo deve fornire un'idea della potenza di questo linguaggio di pre-elaborazione.

## <a name="sass"></a>Sass

Sass è simile a un valore inferiore, che fornisce supporto per molte delle stesse funzionalità, ma con una sintassi leggermente diversa. Viene compilata utilizzando Ruby anziché JavaScript e pertanto dispone di requisiti di installazione diverso. La lingua Sass originale non utilizza le parentesi graffe o punti e virgola, ma invece definita ambito utilizzando lo spazio vuoto e il rientro. Una nuova sintassi introdotta nella versione 3 di Sass, **SCSS** "Sassy CSS ("). SCSS è simile a CSS, in quanto Ignora spazi vuoti e i livelli di rientro e si utilizza un punto e virgola e parentesi graffe.

Per installare Sass, in genere si sarebbe prima di tutto installare Ruby (pre-installato in macOS) e quindi eseguire:

```console
gem install sass
```

Tuttavia, se si esegue Visual Studio, è possibile iniziare a usare Sass in gran parte esattamente come si farebbe con minore. Aprire *package. JSON* e aggiungere il pacchetto "gulp-sass" `devDependencies`:

```json
"devDependencies": {
  "gulp": "3.9.1",
  "gulp-less": "3.3.0",
  "gulp-sass": "3.1.0"
}
```

Successivamente, modificare *gulpfile.js* per aggiungere una variabile sass e un'attività per compilare i file Sass e inserire i risultati nella cartella wwwroot:

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

È ora possibile aggiungere il file Sass *main2.scss* per il *stili* cartella nella radice del progetto:

![aggiungere file scss](less-sass-fa/_static/add-scss-file.png)

Aprire *main2.scss* e aggiungere le seguenti:

```sass
$base: #CC0000;
body {
    background-color: $base;
}
```

Salvare tutti i file. Quando si aggiorna ora **Task Runner Explorer**, vedrai un `sass` attività. Eseguirlo ed esaminare il */wwwroot/css* cartella. È ora disponibile un *main2.css* file, con questo tipo di contenuto:

```css
body {
    background-color: #CC0000;
}
```

Sass supporta l'annidamento nello stesso stato che minore non, fornendo vantaggi simili. File può essere suddiviso dalla funzione e incluso utilizzando il `@import` direttiva:

```sass
@import 'anotherfile';
```

Sass supporta "mixins", nonché l'utilizzo di `@mixin` (parola chiave) per definirli e `@include` per includerli, come in questo esempio dalla [sass lang.com](http://sass-lang.com):

```sass
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box { @include border-radius(10px); }
```

Oltre a "mixins", Sass supporta inoltre il concetto di ereditarietà, consentendo una classe estendere un altro. È concettualmente simile a un parametro mixin, ma risulta meno codice CSS. Viene utilizzata la `@extend` (parola chiave). Per provare "mixins", aggiungere il comando seguente per il *main2.scss* file:

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

Esaminare l'output in *main2.css* dopo l'esecuzione di `sass` attività in **Task Runner Explorer**:

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

Si noti che tutte le proprietà comuni di mixin di avviso vengono ripetute in ogni classe. Il mixin ha un ottimo lavoro di aiutare a evitare la duplicazione in fase di sviluppo, ma viene comunque creata CSS con molta la duplicazione, risultante di dimensioni superiori al file CSS necessari - un potenziale problema di prestazioni.

Sostituire il mixin avviso con un `.alert` classe e modificare `@include` a `@extend` (ricordando di estendere `.alert`, non `alert`):

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

Eseguire ancora una volta Sass ed esaminare il codice risultante CSS:

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

Ora le proprietà vengono definite solo il numero di volte in base alle necessità, e prestazioni migliori in cui viene generato CSS.

Sass include anche le funzioni e operazioni di logica condizionale, simile a un valore inferiore. In effetti, le funzionalità di due linguaggi sono molto simili.

## <a name="less-or-sass"></a>Minore o Sass?

Non è ancora un consenso che indica se è in genere preferibile utilizzare minore o Sass (o anche se preferisce il Sass originale o la sintassi più recente di SCSS in Sass). Probabilmente la decisione più importante consiste nel **utilizzare uno di questi strumenti**, in contrapposizione alla appena-codifica manuale dei file CSS. Dopo aver apportato che delle decisioni, entrambi minore e Sass costituiscono un'ottima scelta.

## <a name="font-awesome"></a>Tipo di carattere straordinario

Oltre ai preprocessori CSS, un'altra risorsa ottima per applicazioni web moderne lo stile è straordinario del carattere. Tipo di carattere amici sono un toolkit che fornisce le icone del vettore scalabile oltre 500 che possono essere usate liberamente in applicazioni web. È stato originariamente progettato per funzionare con Bootstrap, ma non dispone di alcuna dipendenza su tale framework o su tutte le librerie JavaScript.

Il modo più semplice per iniziare con tipo di carattere straordinario consiste nell'aggiungere un riferimento a esso, utilizzando il percorso di rete (CDN) di recapito di contenuto pubblico:

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
```

È possibile anche aggiungerlo al progetto di Visual Studio aggiungendolo al "dipendenze" in *bower. JSON*:

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

Dopo aver creato un riferimento a carattere straordinario in una pagina, è possibile aggiungere le icone all'applicazione tramite l'applicazione di tipo di carattere straordinario classi, in genere è precedute da "fa-", per gli elementi HTML inline (ad esempio `<span>` o `<i>`).  Ad esempio, è possibile aggiungere le icone per elenchi semplici e menu usando codice simile al seguente:

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

Ciò produce il seguente nel browser - si noti l'icona accanto a ogni elemento:

![elenco icone](less-sass-fa/_static/list-icons-screenshot.png)

È possibile visualizzare un elenco completo delle icone disponibili:

http://fontawesome.io/icons/

## <a name="summary"></a>Riepilogo

Applicazioni web moderne richiedono sempre più reattive, fluido progettazioni di pulito, intuitiva e facile da usare da un'ampia gamma di dispositivi. Gestire la complessità di fogli di stile CSS, necessarie per raggiungere questi obiettivi è più adatto utilizzano meno di un tipo per il preprocessore o Sass. Inoltre, Toolkit come tipo di carattere straordinario possa fornire rapidamente le icone note ai menu di navigazione testuale e i pulsanti, l'utente complessiva di migliorare l'esperienza dell'applicazione.
