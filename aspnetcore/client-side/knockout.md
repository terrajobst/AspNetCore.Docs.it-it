---
title: Framework MVVM Knockout.js in ASP.NET Core
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b20e3b23-1c51-47bf-adac-91b5048567e0
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/knockout
ms.openlocfilehash: d1c5cbd430587b757bb550f8f04355e67f04eb54
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="knockoutjs-mvvm-framework-in-aspnet-core"></a>Framework MVVM Knockout.js in ASP.NET Core

Da [Steve Smith](https://ardalis.com/)

Knockout è una libreria JavaScript più diffusa che semplifica la creazione di interfacce utente complesse basate sui dati. E può essere utilizzato da solo o con altre librerie, ad esempio jQuery. Lo scopo principale consiste nell'associare elementi dell'interfaccia utente a un modello di dati sottostante, definito come un oggetto di JavaScript, in modo che quando vengono apportate modifiche all'interfaccia utente, il modello viene aggiornato e viceversa. Knockout semplifica l'utilizzo di un modello Model-View-ViewModel (MVVM) nel comportamento di un'applicazione web sul lato client. I due principali concetti uno necessario sapere quando si lavora con implementazione MVVM del Knockout sono oggetti osservabili e associazioni.

## <a name="getting-started"></a>Per iniziare

Knockout viene distribuito come un unico file JavaScript, pertanto l'installazione e utilizzo è molto semplice e Usa [bower](bower.md). Se si dispone già [bower](bower.md) e [gulp](using-gulp.md) configurato, aprire *bower. JSON* nei componenti di base di ASP.NET del progetto e aggiungere la dipendenza knockout, come illustrato di seguito:

```json
{
  "name": "KnockoutDemo",
  "private": true,
  "dependencies": {
    "knockout" : "^3.3.0"
  },
  "exportsOverride": {
  }
}
```

A questo punto, è possibile quindi manualmente eseguire bower aprendo Task Runner Explorer (in visualizzazione ‣ altre finestre ‣ Task Runner Explorer) e quindi in attività, fare clic su bower e scegliere Esegui. Il risultato dovrebbe essere simile al seguente:

![bower knockout in esecuzione in Esplora esecuzione attività](knockout/_static/bower-knockout.png)

Se si osserva il progetto ora `wwwroot` cartella, dovrebbe essere knockout installato nella cartella lib.

![Knockout installati nella cartella lib](knockout/_static/wwwroot-knockout.png)

È consigliabile che nell'ambiente di produzione si fa riferimento knockout tramite una rete CDN o della rete CDN, come si aumentano le probabilità che gli utenti avranno già una copia memorizzata nella cache del file e pertanto non è necessario scaricarlo affatto. Knockout è disponibile in diverse reti CDN, tra cui Microsoft Ajax CDN, di seguito:

[http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.3.0.js](http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js)

Per includere Knockout in una pagina in uso, aggiungere semplicemente un `<script>` elemento che fa riferimento il file dal punto in cui dovranno essere ospitati, (con l'applicazione o tramite una rete CDN):

```html
<script type="text/javascript" src="knockout-3.3.0.js"></script>
```

## <a name="observables-viewmodels-and-simple-binding"></a>Oggetti osservabili ViewModel e associazione semplice

Potrebbe avere già familiarità con l'utilizzo di JavaScript per modificare gli elementi in una pagina web, tramite l'accesso diretto al DOM o utilizzo di una raccolta, ad esempio jQuery. In genere questo tipo di comportamento avviene mediante la scrittura di codice per impostare direttamente i valori degli elementi in risposta a determinate azioni dell'utente. Con Knockout, un approccio dichiarativo viene eseguito al contrario, tramite il quale gli elementi della pagina sono associati alle proprietà in un oggetto. Invece di scrivere codice per modificare gli elementi DOM, le azioni dell'utente è sufficiente interagiscono con l'oggetto ViewModel e Knockout si occupa di garantire che gli elementi della pagina sono sincronizzati.

Un esempio semplice, prendere in considerazione l'elenco delle pagine seguente. Include un `<span>` elemento con un `data-bind` attributo che indica che il contenuto di testo deve essere associato a autore. Successivamente, in un blocco JavaScript viewModel un variabile viene definito con una singola proprietà, `authorName`, impostata su un valore. Infine, una chiamata a `ko.applyBindings` viene effettuata, il passaggio di questa variabile viewModel.

```html
<html>
<head>
    <script type="text/javascript" src="lib/knockout/knockout.js"></script>
</head>
<body>
    <h1>Some Article</h1>
    <p>
        By <span data-bind="text: authorName"></span>
    </p>
    <script type="text/javascript">
      var viewModel = {
        authorName: 'Steve Smith'
      };
      ko.applyBindings(viewModel);
    </script>
</body>
</html>
```

Quando viene visualizzato nel browser, il contenuto del <span> elemento viene sostituito con il valore nella variabile viewModel:

![associazione semplice Knockout](knockout/_static/simple-binding-screenshot.png)

È ora disponibile lavoro semplice associazione unidirezionale. Si noti che nessuna area nel codice ha creiamo JavaScript per assegnare un valore in base al contenuto dell'intervallo. Se si desidera modificare l'elemento ViewModel, è possibile eseguire un'ulteriore questo un passaggio e aggiungere una casella di input HTML e associare al valore, ad esempio, in modo:

```html
<p>
    Author Name: <input type="text" data-bind="value: authorName" />
</p>
```

Ricaricare la pagina, si può vedere che questo valore è effettivamente associato alla casella di input:

![associazione di input di ritaglio](knockout/_static/input-binding-screenshot.png)

Tuttavia, se si modifica il valore nella casella di testo, il valore corrispondente nel `<span>` elemento non viene modificato. non vengono visualizzati?

Il problema è che non riceve una notifica di `<span>` necessari da aggiornare. Semplicemente aggiornando il ViewModel non autonomamente sufficienti, a meno che le proprietà del ViewModel vengono incapsulate in un tipo speciale. È necessario utilizzare **Observable** nel ViewModel per tutte le proprietà a cui deve essere aggiornate automaticamente quando si verificano modifiche. Modificando ViewModel utilizzare `ko.observable("value")` anziché semplicemente "value", l'elemento ViewModel aggiornerà tutti gli elementi HTML associati al valore ogni volta che viene apportata una modifica. Si noti che le caselle di input non aggiornino il loro valore fino a quando non perdono lo stato attivo, per vedere modifiche a elementi associati durante la digitazione.

> [!NOTE]
> Aggiunta del supporto per l'aggiornamento in tempo reale dopo ogni pressione è sufficiente aggiungere `valueUpdate: "afterkeydown"` per il `data-bind` contenuto dell'attributo. È anche possibile ottenere questo comportamento utilizzando `data-bind="textInput: authorName"` per ottenere gli aggiornamenti immediati di valori. 

Il nostro viewModel, dopo aver aggiornato in modo che utilizzi ko.observable:

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith')
};
ko.applyBindings(viewModel);
```

Knockout supporta un numero di tipi diversi di binding. Finora abbiamo visto come associare a `text` e `value`. È anche possibile associare a qualsiasi attributo specificato. Ad esempio, per creare un collegamento ipertestuale con un tag di ancoraggio, la `src` attributo può essere associato al viewModel. Knockout supporta anche l'associazione alle funzioni. Per dimostrare questo concetto, si aggiornare l'elemento viewModel per includere l'handle twitter dell'autore e visualizzare l'handle twitter come un collegamento alla pagina di twitter dell'autore. Faremo in tre fasi.

In primo luogo, aggiungere il codice HTML per visualizzare il collegamento ipertestuale, vi mostreremo tra parentesi dopo il nome dell'autore:

```html
<h1>Some Article</h1>
<p>
    By <span data-bind="text: authorName"></span>
    (<a data-bind="attr: { href: twitterUrl}, text: twitterAlias" ></a>)
</p>
```

Aggiornare quindi il viewModel per includere le proprietà twitterUrl e twitterAlias:

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith'),
  twitterAlias: ko.observable('@ardalis'),
  twitterUrl: ko.computed(function() {
    return "https://twitter.com/";
  }, this)
};
ko.applyBindings(viewModel);
```

Si noti che a questo punto non è ancora aggiornato twitterUrl per passare all'URL corretto per questo alias twitter: punta appena twitter.com. Si noti inoltre che viene usata una nuova funzione, Knockout `computed`, per twitterUrl. Questa è una funzione observable che notifica eventuali elementi dell'interfaccia utente se viene modificato. Tuttavia, per poter accedere ad altre proprietà di viewModel, è necessario modificare come viewModel, verrà creato in modo che ogni proprietà sono il proprio tipo di istruzione.

Di seguito è riportata la dichiarazione viewModel modificata. A questo punto viene dichiarato come una funzione. Si noti che ogni proprietà sono ora un proprio istruzione che termina con un punto e virgola. Si noti inoltre che per accedere al valore della proprietà twitterAlias, è necessario eseguire l'operazione, pertanto il relativo riferimento include ().

```javascript
function viewModel() {
  this.authorName = ko.observable('Steve Smith');
  this.twitterAlias = ko.observable('@ardalis');

  this.twitterUrl = ko.computed(function() {
    return "https://twitter.com/" + this.twitterAlias().replace('@','');
  }, this)
};
ko.applyBindings(viewModel);
```

Il risultato funziona come previsto nel browser:

![collegamento ipertestuale di ritaglio](knockout/_static/hyperlink-screenshot.png)

Knockout supporta anche l'associazione a determinati eventi di elemento dell'interfaccia utente, ad esempio l'evento click. Ciò consente di associare elementi dell'interfaccia utente semplice e in modo dichiarativo a funzioni all'interno di viewModel dell'applicazione. Un esempio semplice, è possibile aggiungere un pulsante che, quando si fa clic, modifica twitterAlias dell'autore per essere maiuscolo.

In primo luogo, è aggiungere il pulsante, l'associazione per il pulsante evento e fare riferimento al nome di funzione si intende aggiungere l'elemento viewModel:

```html
<p>
    <button data-bind="click: capitalizeTwitterAlias">Capitalize</button>
</p>
```

Quindi, aggiungere la funzione per l'elemento viewModel e collegarla alla modifica dello stato del viewModel. Si noti che per impostare un nuovo valore alla proprietà twitterAlias, abbiamo chiamarlo come metodo e passare il nuovo valore.

```javascript
function viewModel() {
  this.authorName = ko.observable('Steve Smith');
  this.twitterAlias = ko.observable('@ardalis');

  this.twitterUrl = ko.computed(function() {
    return "https://twitter.com/" + this.twitterAlias().replace('@','');
  }, this);

  this.capitalizeTwitterAlias = function() {
    var currentValue = this.twitterAlias();
    this.twitterAlias(currentValue.toUpperCase());
  }
};
ko.applyBindings(viewModel);
```

Esecuzione del codice e fare clic sul pulsante Modifica collegamento visualizzato come previsto:

![Converte in maiuscolo collegamento ipertestuale](knockout/_static/hyperlink-caps-screenshot.png)

## <a name="control-flow"></a>Flusso di controllo

Knockout include le associazioni che è possono eseguire operazioni condizionali e cicli. Le operazioni di ciclo sono particolarmente utili per l'associazione di elenchi di dati per gli elenchi, menu e griglie o tabelle dell'interfaccia utente. L'associazione foreach si ripete su una matrice. Se utilizzato con una matrice observable, verranno aggiornati automaticamente gli elementi dell'interfaccia utente quando gli elementi vengono aggiunti o rimossi dalla matrice, senza creare nuovamente ogni elemento nell'albero dell'interfaccia utente. L'esempio seguente usa un nuovo viewModel che include una matrice dei risultati di giochi observable. È associato a una tabella semplice con due colonne utilizzando un `foreach` vincola il `<tbody>` elemento. Ogni `<tr>` elemento all'interno di `<tbody>` verrà associata a un elemento della raccolta gameResults.

```html
<h1>Record</h1>
<table>
    <thead>
        <tr>
            <th>Opponent</th>
            <th>Result</th>
        </tr>
    </thead>
    <tbody data-bind="foreach: gameResults">
        <tr>
            <td data-bind="text:opponent"></td>
            <td data-bind="text:result"></td>
        </tr>
    </tbody>
</table>
<script type="text/javascript">
  function GameResult(opponent, result) {
    var self = this;
    self.opponent = opponent;
    self.result = ko.observable(result);
  }

  function ViewModel() {
    var self = this;

    self.resultChoices = ["Win", "Loss", "Tie"];

    self.gameResults = ko.observableArray([
      new GameResult("Brendan", self.resultChoices[0]),
      new GameResult("Brendan", self.resultChoices[0]),
      new GameResult("Michelle", self.resultChoices[1])
    ]);
  };
  ko.applyBindings(new ViewModel);
</script>
```

Si noti che in questo caso si utilizza ViewModel con una lettera maiuscola "V" perché si prevede di creare l'oggetto utilizzando il "nuovo" (nella chiamata applyBindings). Quando viene eseguita, la pagina produce il seguente output:

![modello di visualizzazione di record di ritaglio](knockout/_static/record-screenshot.png)

Per dimostrare il funzionamento raccolta osservabile, aggiungere un po' più funzionalità. È possibile includono la possibilità di registrare i risultati di un altro gioco per l'elemento ViewModel e quindi aggiungere un pulsante e un'interfaccia utente per funzionare con questa nuova funzione.  Innanzitutto, creare il metodo addResult:

```javascript
// add this to ViewModel()
self.addResult = function() {
  self.gameResults.push(new GameResult("", self.resultChoices[0]));
}
```

Associare a questo metodo di un pulsante tramite il `click` associazione:

```html
<button data-bind="click: addResult">Add New Result</button>
```

Aprire la pagina nel browser e fare clic sul pulsante un paio di volte, risultante in una nuova riga di tabella con ogni clic:

![Aggiungere i risultati](knockout/_static/record-addresult-screenshot.png)

Esistono alcuni modi per supportare l'aggiunta di nuovi record nell'interfaccia utente, in genere inline o in un modulo separato. È possibile modificare facilmente la tabella per l'utilizzo di caselle di testo e controlli DropDownList in modo che tutto sia modificabile. Modificare solo il `<tr>` elemento, come illustrato:

```html
<tbody data-bind="foreach: gameResults">
    <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
    </tr>
</tbody>
```

Si noti che `$root` fa riferimento alla radice ViewModel, ovvero in cui vengono esposte le scelte possibili. `$data`Se si intende qualsiasi il modello corrente è all'interno di un contesto specifico, in questo caso che fa riferimento a un singolo elemento della matrice resultChoices, ognuno dei quali è una stringa semplice.

Con questa modifica, l'intera griglia diventa modificabile:

![Griglia modificabile](knockout/_static/editable-grid-screenshot.png)

Se non stessimo utilizzando Knockout, è infatti possibile ottenere tutte queste tramite jQuery, ma è probabile che non verrebbe essere quasi efficace. Knockout tiene traccia di dati associati l'elemento ViewModel elementi corrispondono agli elementi dell'interfaccia utente e vengono aggiornati solo gli elementi che devono essere aggiunti, rimossi o aggiornati. Richiederebbe notevole impegno per ottenere questo risultato effettuata tramite jQuery o la modifica diretta DOM e anche in questo caso se Desideravamo quindi visualizzare i risultati aggregati (ad esempio un record di perdita win) in base ai dati della tabella, è necessario ancora una volta ciclo e analizzare il Elementi HTML.  Con Knockout, la visualizzazione del record win perdita è piuttosto semplice. È possibile eseguire i calcoli all'interno di ViewModel stesso e quindi visualizzarla con un'associazione di testo semplice e un `<span>`.

Per compilare la stringa di record win perdita, è possibile usare un observable calcolata. Si noti che i riferimenti alle proprietà observable all'interno di ViewModel devono essere chiamate di funzione, in caso contrario non si recupererà il valore di observable (ad esempio `gameResults()` non `gameResults` nel codice visualizzato):

```javascript
self.displayRecord = ko.computed(function () {
  var wins = self.gameResults().filter(function (value) { return value.result() == "Win"; }).length;
  var losses = self.gameResults().filter(function (value) { return value.result() == "Loss"; }).length;
  var ties = self.gameResults().filter(function (value) { return value.result() == "Tie"; }).length;
  return wins + " - " + losses + " - " + ties;
}, this);
```

Associare questa funzione a un intervallo all'interno di `<h1>` elemento nella parte superiore della pagina:

```html
<h1>Record <span data-bind="text: displayRecord"></span></h1>
```

il risultato:

![Perdita di trattative concluse](knockout/_static/record-winloss-screenshot.png)

Aggiunge righe o si modifica l'elemento selezionato nella colonna di risultati di qualsiasi riga verrà aggiornati al record visualizzato nella parte superiore della finestra.

Oltre l'associazione a valori, è anche possibile usare qualsiasi espressione JavaScript validi all'interno di un'associazione. Ad esempio, se un elemento dell'interfaccia utente dovrebbe essere visualizzato solo in determinate condizioni, ad esempio quando un valore supera una determinata soglia, è possibile specificare questo logicamente all'interno dell'espressione di associazione:

```html
<div data-bind="visible: customerValue > 100"></div>
```

Questo `<div>` saranno visibili solo quando il customerValue maggiori di 100.

## <a name="templates"></a>Modelli

Knockout dispone di supporto per i modelli, che è possibile separare facilmente l'interfaccia utente da un comportamento di o caricare in modo incrementale gli elementi dell'interfaccia utente in un'applicazione di grandi dimensioni su richiesta. È possibile aggiornare l'esempio precedente per rendere il proprio modello di ogni riga semplicemente estrazione HTML out in un modello e specificando il modello in base al nome nella chiamata di un'associazione dati in `<tbody>`.

```html
<tbody data-bind="template: { name: 'rowTemplate', foreach: gameResults }">
</tbody>
<script type="text/html" id="rowTemplate">
  <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
  </tr>
</script>
```

Knockout supporta anche altri motori di modelli, ad esempio la libreria jQuery.tmpl e motore di Underscore.js modello.

## <a name="components"></a>Componenti

Componenti consentono di organizzare e riutilizzare il codice dell'interfaccia utente, in genere insieme ai dati ViewModel da cui dipende il codice dell'interfaccia utente. Per creare un componente, è sufficiente specificare il modello e il relativo viewModel e assegnargli un nome. Questa operazione viene effettuata chiamando `ko.components.register()`. Inoltre, per definire i modelli e viewmodel inline, possono essere caricati da file esterni mediante una libreria come *require.js*, con conseguente codice molto pulito ed efficiente.

## <a name="communicating-with-apis"></a>La comunicazione con le API

Knockout lavorare con i dati in formato JSON. È un modo comune per recuperare e salvare i dati usando Knockout con jQuery, che supporta il `$.getJSON()` funzione per recuperare i dati e `$.post()` metodo per inviare dati dal browser a un endpoint dell'API. Naturalmente, se si preferisce un modo diverso per inviare e ricevere i dati JSON, Knockout funzionerà con esso anche.

## <a name="summary"></a>Riepilogo

Knockout fornisce un modo semplice ed elegante per associare lo stato corrente dell'applicazione client, definito in un elemento ViewModel di elementi dell'interfaccia utente. La sintassi di associazione del Knockout utilizza l'attributo data-bind, applicato agli elementi HTML che devono essere elaborati. È in grado di eseguire il rendering in modo efficiente e aggiornare i set di dati di grandi dimensioni grazie al rilevamento di elementi dell'interfaccia utente Knockout e solo l'elaborazione delle modifiche per interessati elementi. Applicazioni di grandi dimensioni possono suddividere la logica dell'interfaccia utente mediante modelli e componenti, che possono essere caricati su richiesta da file esterni. Attualmente versione 3, Knockout è una libreria JavaScript stabile che può migliorare le applicazioni web che richiedono di interattività rich client.
