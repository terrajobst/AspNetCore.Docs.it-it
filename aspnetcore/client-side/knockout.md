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
ms.openlocfilehash: 87b4fdc86f6bb870ae0a8cc85688a549fd0740ac
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="knockoutjs-mvvm-framework-in-aspnet-core"></a><span data-ttu-id="8297e-103">Framework MVVM Knockout.js in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8297e-103">Knockout.js MVVM Framework in ASP.NET Core</span></span>

<span data-ttu-id="8297e-104">Da [Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="8297e-104">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="8297e-105">Knockout è una libreria JavaScript più diffusa che semplifica la creazione di interfacce utente complesse basate sui dati.</span><span class="sxs-lookup"><span data-stu-id="8297e-105">Knockout is a popular JavaScript library that simplifies the creation of complex data-based user interfaces.</span></span> <span data-ttu-id="8297e-106">E può essere utilizzato da solo o con altre librerie, ad esempio jQuery.</span><span class="sxs-lookup"><span data-stu-id="8297e-106">It can be used alone or with other libraries, such as jQuery.</span></span> <span data-ttu-id="8297e-107">Lo scopo principale consiste nell'associare elementi dell'interfaccia utente a un modello di dati sottostante, definito come un oggetto di JavaScript, in modo che quando vengono apportate modifiche all'interfaccia utente, il modello viene aggiornato e viceversa.</span><span class="sxs-lookup"><span data-stu-id="8297e-107">Its primary purpose is to bind UI elements to an underlying data model defined as a JavaScript object, such that when changes are made to the UI, the model is updated, and vice versa.</span></span> <span data-ttu-id="8297e-108">Knockout semplifica l'utilizzo di un modello Model-View-ViewModel (MVVM) nel comportamento di un'applicazione web sul lato client.</span><span class="sxs-lookup"><span data-stu-id="8297e-108">Knockout facilitates the use of a Model-View-ViewModel (MVVM) pattern in a web application's client-side behavior.</span></span> <span data-ttu-id="8297e-109">I due principali concetti uno necessario sapere quando si lavora con implementazione MVVM del Knockout sono oggetti osservabili e associazioni.</span><span class="sxs-lookup"><span data-stu-id="8297e-109">The two main concepts one must learn when working with Knockout's MVVM implementation are Observables and Bindings.</span></span>

## <a name="getting-started"></a><span data-ttu-id="8297e-110">Per iniziare</span><span class="sxs-lookup"><span data-stu-id="8297e-110">Getting started</span></span>

<span data-ttu-id="8297e-111">Knockout viene distribuito come un unico file JavaScript, pertanto l'installazione e utilizzo è molto semplice e Usa [bower](bower.md).</span><span class="sxs-lookup"><span data-stu-id="8297e-111">Knockout is deployed as a single JavaScript file, so installing and using it is very straightforward using [bower](bower.md).</span></span> <span data-ttu-id="8297e-112">Se si dispone già [bower](bower.md) e [gulp](using-gulp.md) configurato, aprire *bower. JSON* nei componenti di base di ASP.NET del progetto e aggiungere la dipendenza knockout, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="8297e-112">Assuming you already have [bower](bower.md) and [gulp](using-gulp.md) configured, open *bower.json* in your ASP.NET Core project and add the knockout dependency as shown here:</span></span>

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

<span data-ttu-id="8297e-113">A questo punto, è possibile quindi manualmente eseguire bower aprendo Task Runner Explorer (in visualizzazione ‣ altre finestre ‣ Task Runner Explorer) e quindi in attività, fare clic su bower e scegliere Esegui.</span><span class="sxs-lookup"><span data-stu-id="8297e-113">With this in place, you can then manually run bower by opening the Task Runner Explorer (under View ‣ Other Windows ‣ Task Runner Explorer) and then under Tasks, right-click on bower and select Run.</span></span> <span data-ttu-id="8297e-114">Il risultato dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="8297e-114">The result should appear similar to this:</span></span>

![bower knockout in esecuzione in Esplora esecuzione attività](knockout/_static/bower-knockout.png)

<span data-ttu-id="8297e-116">Se si osserva il progetto ora `wwwroot` cartella, dovrebbe essere knockout installato nella cartella lib.</span><span class="sxs-lookup"><span data-stu-id="8297e-116">Now if you look in your project's `wwwroot` folder, you should see knockout installed under the lib folder.</span></span>

![Knockout installati nella cartella lib](knockout/_static/wwwroot-knockout.png)

<span data-ttu-id="8297e-118">È consigliabile che nell'ambiente di produzione si fa riferimento knockout tramite una rete CDN o della rete CDN, come si aumentano le probabilità che gli utenti avranno già una copia memorizzata nella cache del file e pertanto non è necessario scaricarlo affatto.</span><span class="sxs-lookup"><span data-stu-id="8297e-118">It's recommended that in your production environment you reference knockout via a Content Delivery Network, or CDN, as this increases the likelihood that your users will already have a cached copy of the file and thus will not need to download it at all.</span></span> <span data-ttu-id="8297e-119">Knockout è disponibile in diverse reti CDN, tra cui Microsoft Ajax CDN, di seguito:</span><span class="sxs-lookup"><span data-stu-id="8297e-119">Knockout is available on several CDNs, including the Microsoft Ajax CDN, here:</span></span>

[<span data-ttu-id="8297e-120">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.3.0.js</span><span class="sxs-lookup"><span data-stu-id="8297e-120">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span></span>](http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js)

<span data-ttu-id="8297e-121">Per includere Knockout in una pagina in uso, aggiungere semplicemente un `<script>` elemento che fa riferimento il file dal punto in cui dovranno essere ospitati, (con l'applicazione o tramite una rete CDN):</span><span class="sxs-lookup"><span data-stu-id="8297e-121">To include Knockout on a page that will use it, simply add a `<script>` element referencing the file from wherever you will be hosting it (with your application, or via a CDN):</span></span>

```html
<script type="text/javascript" src="knockout-3.3.0.js"></script>
```

## <a name="observables-viewmodels-and-simple-binding"></a><span data-ttu-id="8297e-122">Oggetti osservabili ViewModel e associazione semplice</span><span class="sxs-lookup"><span data-stu-id="8297e-122">Observables, ViewModels, and simple binding</span></span>

<span data-ttu-id="8297e-123">Potrebbe avere già familiarità con l'utilizzo di JavaScript per modificare gli elementi in una pagina web, tramite l'accesso diretto al DOM o utilizzo di una raccolta, ad esempio jQuery.</span><span class="sxs-lookup"><span data-stu-id="8297e-123">You may already be familiar with using JavaScript to manipulate elements on a web page, either via direct access to the DOM or using a library like jQuery.</span></span> <span data-ttu-id="8297e-124">In genere questo tipo di comportamento avviene mediante la scrittura di codice per impostare direttamente i valori degli elementi in risposta a determinate azioni dell'utente.</span><span class="sxs-lookup"><span data-stu-id="8297e-124">Typically this kind of behavior is achieved by writing code to directly set element values in response to certain user actions.</span></span> <span data-ttu-id="8297e-125">Con Knockout, un approccio dichiarativo viene eseguito al contrario, tramite il quale gli elementi della pagina sono associati alle proprietà in un oggetto.</span><span class="sxs-lookup"><span data-stu-id="8297e-125">With Knockout, a declarative approach is taken instead, through which elements on the page are bound to properties on an object.</span></span> <span data-ttu-id="8297e-126">Invece di scrivere codice per modificare gli elementi DOM, le azioni dell'utente è sufficiente interagiscono con l'oggetto ViewModel e Knockout si occupa di garantire che gli elementi della pagina sono sincronizzati.</span><span class="sxs-lookup"><span data-stu-id="8297e-126">Instead of writing code to manipulate DOM elements, user actions simply interact with the ViewModel object, and Knockout takes care of ensuring the page elements are synchronized.</span></span>

<span data-ttu-id="8297e-127">Un esempio semplice, prendere in considerazione l'elenco delle pagine seguente.</span><span class="sxs-lookup"><span data-stu-id="8297e-127">As a simple example, consider the page list below.</span></span> <span data-ttu-id="8297e-128">Include un `<span>` elemento con un `data-bind` attributo che indica che il contenuto di testo deve essere associato a autore.</span><span class="sxs-lookup"><span data-stu-id="8297e-128">It includes a `<span>` element with a `data-bind` attribute indicating that the text content should be bound to authorName.</span></span> <span data-ttu-id="8297e-129">Successivamente, in un blocco JavaScript viewModel un variabile viene definito con una singola proprietà, `authorName`, impostata su un valore.</span><span class="sxs-lookup"><span data-stu-id="8297e-129">Next, in a JavaScript block a variable viewModel is defined with a single property, `authorName`, set to some value.</span></span> <span data-ttu-id="8297e-130">Infine, una chiamata a `ko.applyBindings` viene effettuata, il passaggio di questa variabile viewModel.</span><span class="sxs-lookup"><span data-stu-id="8297e-130">Finally, a call to `ko.applyBindings` is made, passing in this viewModel variable.</span></span>

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

<span data-ttu-id="8297e-131">Quando viene visualizzato nel browser, il contenuto del <span> elemento viene sostituito con il valore nella variabile viewModel:</span><span class="sxs-lookup"><span data-stu-id="8297e-131">When viewed in the browser, the content of the <span> element is replaced with the value in the viewModel variable:</span></span>

![associazione semplice Knockout](knockout/_static/simple-binding-screenshot.png)

<span data-ttu-id="8297e-133">È ora disponibile lavoro semplice associazione unidirezionale.</span><span class="sxs-lookup"><span data-stu-id="8297e-133">We now have simple one-way binding working.</span></span> <span data-ttu-id="8297e-134">Si noti che nessuna area nel codice ha creiamo JavaScript per assegnare un valore in base al contenuto dell'intervallo.</span><span class="sxs-lookup"><span data-stu-id="8297e-134">Notice that nowhere in the code did we write JavaScript to assign a value to the span's contents.</span></span> <span data-ttu-id="8297e-135">Se si desidera modificare l'elemento ViewModel, è possibile eseguire un'ulteriore questo un passaggio e aggiungere una casella di input HTML e associare al valore, ad esempio, in modo:</span><span class="sxs-lookup"><span data-stu-id="8297e-135">If we want to manipulate the ViewModel, we can take this a step further and add an HTML input textbox, and bind to its value, like so:</span></span>

```html
<p>
    Author Name: <input type="text" data-bind="value: authorName" />
</p>
```

<span data-ttu-id="8297e-136">Ricaricare la pagina, si può vedere che questo valore è effettivamente associato alla casella di input:</span><span class="sxs-lookup"><span data-stu-id="8297e-136">Reloading the page, we see that this value is indeed bound to the input box:</span></span>

![associazione di input di ritaglio](knockout/_static/input-binding-screenshot.png)

<span data-ttu-id="8297e-138">Tuttavia, se si modifica il valore nella casella di testo, il valore corrispondente nel `<span>` elemento non viene modificato.</span><span class="sxs-lookup"><span data-stu-id="8297e-138">However, if we change the value in the textbox, the corresponding value in the `<span>` element doesn't change.</span></span> <span data-ttu-id="8297e-139">non vengono visualizzati?</span><span class="sxs-lookup"><span data-stu-id="8297e-139">Why not?</span></span>

<span data-ttu-id="8297e-140">Il problema è che non riceve una notifica di `<span>` necessari da aggiornare.</span><span class="sxs-lookup"><span data-stu-id="8297e-140">The issue is that nothing notified the `<span>` that it needed to be updated.</span></span> <span data-ttu-id="8297e-141">Semplicemente aggiornando il ViewModel non autonomamente sufficienti, a meno che le proprietà del ViewModel vengono incapsulate in un tipo speciale.</span><span class="sxs-lookup"><span data-stu-id="8297e-141">Simply updating the ViewModel isn't by itself sufficient, unless the ViewModel's properties are wrapped in a special type.</span></span> <span data-ttu-id="8297e-142">È necessario utilizzare **Observable** nel ViewModel per tutte le proprietà a cui deve essere aggiornate automaticamente quando si verificano modifiche.</span><span class="sxs-lookup"><span data-stu-id="8297e-142">We need to use **observables** in the ViewModel for any properties that need to have changes automatically updated as they occur.</span></span> <span data-ttu-id="8297e-143">Modificando ViewModel utilizzare `ko.observable("value")` anziché semplicemente "value", l'elemento ViewModel aggiornerà tutti gli elementi HTML associati al valore ogni volta che viene apportata una modifica.</span><span class="sxs-lookup"><span data-stu-id="8297e-143">By changing the ViewModel to use `ko.observable("value")` instead of just "value", the ViewModel will update any HTML elements that are bound to its value whenever a change occurs.</span></span> <span data-ttu-id="8297e-144">Si noti che le caselle di input non aggiornino il loro valore fino a quando non perdono lo stato attivo, per vedere modifiche a elementi associati durante la digitazione.</span><span class="sxs-lookup"><span data-stu-id="8297e-144">Note that input boxes don't update their value until they lose focus, so you won't see changes to bound elements as you type.</span></span>

> [!NOTE]
> <span data-ttu-id="8297e-145">Aggiunta del supporto per l'aggiornamento in tempo reale dopo ogni pressione è sufficiente aggiungere `valueUpdate: "afterkeydown"` per il `data-bind` contenuto dell'attributo.</span><span class="sxs-lookup"><span data-stu-id="8297e-145">Adding support for live updating after each keypress is simply a matter of adding `valueUpdate: "afterkeydown"` to the `data-bind` attribute's contents.</span></span> <span data-ttu-id="8297e-146">È anche possibile ottenere questo comportamento utilizzando `data-bind="textInput: authorName"` per ottenere gli aggiornamenti immediati di valori.</span><span class="sxs-lookup"><span data-stu-id="8297e-146">You can also get this behavior by using `data-bind="textInput: authorName"` to get instant updates of values.</span></span> 

<span data-ttu-id="8297e-147">Il nostro viewModel, dopo aver aggiornato in modo che utilizzi ko.observable:</span><span class="sxs-lookup"><span data-stu-id="8297e-147">Our viewModel, after updating it to use ko.observable:</span></span>

```javascript
var viewModel = {
  authorName: ko.observable('Steve Smith')
};
ko.applyBindings(viewModel);
```

<span data-ttu-id="8297e-148">Knockout supporta un numero di tipi diversi di binding.</span><span class="sxs-lookup"><span data-stu-id="8297e-148">Knockout supports a number of different kinds of bindings.</span></span> <span data-ttu-id="8297e-149">Finora abbiamo visto come associare a `text` e `value`.</span><span class="sxs-lookup"><span data-stu-id="8297e-149">So far we've seen how to bind to `text` and to `value`.</span></span> <span data-ttu-id="8297e-150">È anche possibile associare a qualsiasi attributo specificato.</span><span class="sxs-lookup"><span data-stu-id="8297e-150">You can also bind to any given attribute.</span></span> <span data-ttu-id="8297e-151">Ad esempio, per creare un collegamento ipertestuale con un tag di ancoraggio, la `src` attributo può essere associato al viewModel.</span><span class="sxs-lookup"><span data-stu-id="8297e-151">For instance, to create a hyperlink with an anchor tag, the `src` attribute can be bound to the viewModel.</span></span> <span data-ttu-id="8297e-152">Knockout supporta anche l'associazione alle funzioni.</span><span class="sxs-lookup"><span data-stu-id="8297e-152">Knockout also supports binding to functions.</span></span> <span data-ttu-id="8297e-153">Per dimostrare questo concetto, si aggiornare l'elemento viewModel per includere l'handle twitter dell'autore e visualizzare l'handle twitter come un collegamento alla pagina di twitter dell'autore.</span><span class="sxs-lookup"><span data-stu-id="8297e-153">To demonstrate this, let's update the viewModel to include the author's twitter handle, and display the twitter handle as a link to the author's twitter page.</span></span> <span data-ttu-id="8297e-154">Faremo in tre fasi.</span><span class="sxs-lookup"><span data-stu-id="8297e-154">We'll do this in three stages.</span></span>

<span data-ttu-id="8297e-155">In primo luogo, aggiungere il codice HTML per visualizzare il collegamento ipertestuale, vi mostreremo tra parentesi dopo il nome dell'autore:</span><span class="sxs-lookup"><span data-stu-id="8297e-155">First, add the HTML to display the hyperlink, which we'll show in parentheses after the author's name:</span></span>

```html
<h1>Some Article</h1>
<p>
    By <span data-bind="text: authorName"></span>
    (<a data-bind="attr: { href: twitterUrl}, text: twitterAlias" ></a>)
</p>
```

<span data-ttu-id="8297e-156">Aggiornare quindi il viewModel per includere le proprietà twitterUrl e twitterAlias:</span><span class="sxs-lookup"><span data-stu-id="8297e-156">Next, update the viewModel to include the twitterUrl and twitterAlias properties:</span></span>

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

<span data-ttu-id="8297e-157">Si noti che a questo punto non è ancora aggiornato twitterUrl per passare all'URL corretto per questo alias twitter: punta appena twitter.com.</span><span class="sxs-lookup"><span data-stu-id="8297e-157">Notice that at this point we haven't yet updated the twitterUrl to go to the correct URL for this twitter alias – it's just pointing at twitter.com.</span></span> <span data-ttu-id="8297e-158">Si noti inoltre che viene usata una nuova funzione, Knockout `computed`, per twitterUrl.</span><span class="sxs-lookup"><span data-stu-id="8297e-158">Also notice that we're using a new Knockout function, `computed`, for twitterUrl.</span></span> <span data-ttu-id="8297e-159">Questa è una funzione observable che notifica eventuali elementi dell'interfaccia utente se viene modificato.</span><span class="sxs-lookup"><span data-stu-id="8297e-159">This is an observable function that will notify any UI elements if it changes.</span></span> <span data-ttu-id="8297e-160">Tuttavia, per poter accedere ad altre proprietà di viewModel, è necessario modificare come viewModel, verrà creato in modo che ogni proprietà sono il proprio tipo di istruzione.</span><span class="sxs-lookup"><span data-stu-id="8297e-160">However, for it to have access to other properties in the viewModel, we need to change how we are creating the viewModel, so that each property is its own statement.</span></span>

<span data-ttu-id="8297e-161">Di seguito è riportata la dichiarazione viewModel modificata.</span><span class="sxs-lookup"><span data-stu-id="8297e-161">The revised viewModel declaration is shown below.</span></span> <span data-ttu-id="8297e-162">A questo punto viene dichiarato come una funzione.</span><span class="sxs-lookup"><span data-stu-id="8297e-162">It is now declared as a function.</span></span> <span data-ttu-id="8297e-163">Si noti che ogni proprietà sono ora un proprio istruzione che termina con un punto e virgola.</span><span class="sxs-lookup"><span data-stu-id="8297e-163">Notice that each property is its own statement now, ending with a semicolon.</span></span> <span data-ttu-id="8297e-164">Si noti inoltre che per accedere al valore della proprietà twitterAlias, è necessario eseguire l'operazione, pertanto il relativo riferimento include ().</span><span class="sxs-lookup"><span data-stu-id="8297e-164">Also notice that to access the twitterAlias property value, we need to execute it, so its reference includes ().</span></span>

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

<span data-ttu-id="8297e-165">Il risultato funziona come previsto nel browser:</span><span class="sxs-lookup"><span data-stu-id="8297e-165">The result works as expected in the browser:</span></span>

![collegamento ipertestuale di ritaglio](knockout/_static/hyperlink-screenshot.png)

<span data-ttu-id="8297e-167">Knockout supporta anche l'associazione a determinati eventi di elemento dell'interfaccia utente, ad esempio l'evento click.</span><span class="sxs-lookup"><span data-stu-id="8297e-167">Knockout also supports binding to certain UI element events, such as the click event.</span></span> <span data-ttu-id="8297e-168">Ciò consente di associare elementi dell'interfaccia utente semplice e in modo dichiarativo a funzioni all'interno di viewModel dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8297e-168">This allows you to easily and declaratively bind UI elements to functions within the application's viewModel.</span></span> <span data-ttu-id="8297e-169">Un esempio semplice, è possibile aggiungere un pulsante che, quando si fa clic, modifica twitterAlias dell'autore per essere maiuscolo.</span><span class="sxs-lookup"><span data-stu-id="8297e-169">As a simple example, we can add a button that, when clicked, modifies the author's twitterAlias to be all caps.</span></span>

<span data-ttu-id="8297e-170">In primo luogo, è aggiungere il pulsante, l'associazione per il pulsante evento e fare riferimento al nome di funzione si intende aggiungere l'elemento viewModel:</span><span class="sxs-lookup"><span data-stu-id="8297e-170">First, we add the button, binding to the button's click event, and referencing the function name we're going to add to the viewModel:</span></span>

```html
<p>
    <button data-bind="click: capitalizeTwitterAlias">Capitalize</button>
</p>
```

<span data-ttu-id="8297e-171">Quindi, aggiungere la funzione per l'elemento viewModel e collegarla alla modifica dello stato del viewModel.</span><span class="sxs-lookup"><span data-stu-id="8297e-171">Then, add the function to the viewModel, and wire it up to modify the viewModel's state.</span></span> <span data-ttu-id="8297e-172">Si noti che per impostare un nuovo valore alla proprietà twitterAlias, abbiamo chiamarlo come metodo e passare il nuovo valore.</span><span class="sxs-lookup"><span data-stu-id="8297e-172">Notice that to set a new value to the twitterAlias property, we call it as a method and pass in the new value.</span></span>

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

<span data-ttu-id="8297e-173">Esecuzione del codice e fare clic sul pulsante Modifica collegamento visualizzato come previsto:</span><span class="sxs-lookup"><span data-stu-id="8297e-173">Running the code and clicking the button modifies the displayed link as expected:</span></span>

![Converte in maiuscolo collegamento ipertestuale](knockout/_static/hyperlink-caps-screenshot.png)

## <a name="control-flow"></a><span data-ttu-id="8297e-175">Flusso di controllo</span><span class="sxs-lookup"><span data-stu-id="8297e-175">Control flow</span></span>

<span data-ttu-id="8297e-176">Knockout include le associazioni che è possono eseguire operazioni condizionali e cicli.</span><span class="sxs-lookup"><span data-stu-id="8297e-176">Knockout includes bindings that can perform conditional and looping operations.</span></span> <span data-ttu-id="8297e-177">Le operazioni di ciclo sono particolarmente utili per l'associazione di elenchi di dati per gli elenchi, menu e griglie o tabelle dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="8297e-177">Looping operations are especially useful for binding lists of data to UI lists, menus, and grids or tables.</span></span> <span data-ttu-id="8297e-178">L'associazione foreach si ripete su una matrice.</span><span class="sxs-lookup"><span data-stu-id="8297e-178">The foreach binding will iterate over an array.</span></span> <span data-ttu-id="8297e-179">Se utilizzato con una matrice observable, verranno aggiornati automaticamente gli elementi dell'interfaccia utente quando gli elementi vengono aggiunti o rimossi dalla matrice, senza creare nuovamente ogni elemento nell'albero dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="8297e-179">When used with an observable array, it will automatically update the UI elements when items are added or removed from the array, without re-creating every element in the UI tree.</span></span> <span data-ttu-id="8297e-180">L'esempio seguente usa un nuovo viewModel che include una matrice dei risultati di giochi observable.</span><span class="sxs-lookup"><span data-stu-id="8297e-180">The following example uses a new viewModel which includes an observable array of game results.</span></span> <span data-ttu-id="8297e-181">È associato a una tabella semplice con due colonne utilizzando un `foreach` vincola il `<tbody>` elemento.</span><span class="sxs-lookup"><span data-stu-id="8297e-181">It is bound to a simple table with two columns using a `foreach` binding on the `<tbody>` element.</span></span> <span data-ttu-id="8297e-182">Ogni `<tr>` elemento all'interno di `<tbody>` verrà associata a un elemento della raccolta gameResults.</span><span class="sxs-lookup"><span data-stu-id="8297e-182">Each `<tr>` element within `<tbody>` will be bound to an element of the gameResults collection.</span></span>

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

<span data-ttu-id="8297e-183">Si noti che in questo caso si utilizza ViewModel con una lettera maiuscola "V" perché si prevede di creare l'oggetto utilizzando il "nuovo" (nella chiamata applyBindings).</span><span class="sxs-lookup"><span data-stu-id="8297e-183">Notice that this time we're using ViewModel with a capital “V" because we expect to construct it using “new" (in the applyBindings call).</span></span> <span data-ttu-id="8297e-184">Quando viene eseguita, la pagina produce il seguente output:</span><span class="sxs-lookup"><span data-stu-id="8297e-184">When executed, the page results in the following output:</span></span>

![modello di visualizzazione di record di ritaglio](knockout/_static/record-screenshot.png)

<span data-ttu-id="8297e-186">Per dimostrare il funzionamento raccolta osservabile, aggiungere un po' più funzionalità.</span><span class="sxs-lookup"><span data-stu-id="8297e-186">To demonstrate that the observable collection is working, let's add a bit more functionality.</span></span> <span data-ttu-id="8297e-187">È possibile includono la possibilità di registrare i risultati di un altro gioco per l'elemento ViewModel e quindi aggiungere un pulsante e un'interfaccia utente per funzionare con questa nuova funzione.</span><span class="sxs-lookup"><span data-stu-id="8297e-187">We can include the ability to record the results of another game to the ViewModel, and then add a button and some UI to work with this new function.</span></span>  <span data-ttu-id="8297e-188">Innanzitutto, creare il metodo addResult:</span><span class="sxs-lookup"><span data-stu-id="8297e-188">First, let's create the addResult method:</span></span>

```javascript
// add this to ViewModel()
self.addResult = function() {
  self.gameResults.push(new GameResult("", self.resultChoices[0]));
}
```

<span data-ttu-id="8297e-189">Associare a questo metodo di un pulsante tramite il `click` associazione:</span><span class="sxs-lookup"><span data-stu-id="8297e-189">Bind this method to a button using the `click` binding:</span></span>

```html
<button data-bind="click: addResult">Add New Result</button>
```

<span data-ttu-id="8297e-190">Aprire la pagina nel browser e fare clic sul pulsante un paio di volte, risultante in una nuova riga di tabella con ogni clic:</span><span class="sxs-lookup"><span data-stu-id="8297e-190">Open the page in the browser and click the button a couple of times, resulting in a new table row with each click:</span></span>

![Aggiungere i risultati](knockout/_static/record-addresult-screenshot.png)

<span data-ttu-id="8297e-192">Esistono alcuni modi per supportare l'aggiunta di nuovi record nell'interfaccia utente, in genere inline o in un modulo separato.</span><span class="sxs-lookup"><span data-stu-id="8297e-192">There are a few ways to support adding new records in the UI, typically either inline or in a separate form.</span></span> <span data-ttu-id="8297e-193">È possibile modificare facilmente la tabella per l'utilizzo di caselle di testo e controlli DropDownList in modo che tutto sia modificabile.</span><span class="sxs-lookup"><span data-stu-id="8297e-193">We can easily modify the table to use textboxes and dropdownlists so that the whole thing is editable.</span></span> <span data-ttu-id="8297e-194">Modificare solo il `<tr>` elemento, come illustrato:</span><span class="sxs-lookup"><span data-stu-id="8297e-194">Just change the `<tr>` element as shown:</span></span>

```html
<tbody data-bind="foreach: gameResults">
    <tr>
      <td><input data-bind="value:opponent" /></td>
      <td><select data-bind="options: $root.resultChoices, value:result, optionsText: $data"></select></td>
    </tr>
</tbody>
```

<span data-ttu-id="8297e-195">Si noti che `$root` fa riferimento alla radice ViewModel, ovvero in cui vengono esposte le scelte possibili.</span><span class="sxs-lookup"><span data-stu-id="8297e-195">Note that `$root` refers to the root ViewModel, which is where the possible choices are exposed.</span></span> <span data-ttu-id="8297e-196">`$data`Se si intende qualsiasi il modello corrente è all'interno di un contesto specifico, in questo caso che fa riferimento a un singolo elemento della matrice resultChoices, ognuno dei quali è una stringa semplice.</span><span class="sxs-lookup"><span data-stu-id="8297e-196">`$data` refers to whatever the current model is within a given context - in this case it refers to an individual element of the resultChoices array, each of which is a simple string.</span></span>

<span data-ttu-id="8297e-197">Con questa modifica, l'intera griglia diventa modificabile:</span><span class="sxs-lookup"><span data-stu-id="8297e-197">With this change, the entire grid becomes editable:</span></span>

![Griglia modificabile](knockout/_static/editable-grid-screenshot.png)

<span data-ttu-id="8297e-199">Se non stessimo utilizzando Knockout, è infatti possibile ottenere tutte queste tramite jQuery, ma è probabile che non verrebbe essere quasi efficace.</span><span class="sxs-lookup"><span data-stu-id="8297e-199">If we weren't using Knockout, we could achieve all of this using jQuery, but most likely it would not be nearly as efficient.</span></span> <span data-ttu-id="8297e-200">Knockout tiene traccia di dati associati l'elemento ViewModel elementi corrispondono agli elementi dell'interfaccia utente e vengono aggiornati solo gli elementi che devono essere aggiunti, rimossi o aggiornati.</span><span class="sxs-lookup"><span data-stu-id="8297e-200">Knockout tracks which bound data items in the ViewModel correspond to which UI elements, and only updates those elements that need to be added, removed, or updated.</span></span> <span data-ttu-id="8297e-201">Richiederebbe notevole impegno per ottenere questo risultato effettuata tramite jQuery o la modifica diretta DOM e anche in questo caso se Desideravamo quindi visualizzare i risultati aggregati (ad esempio un record di perdita win) in base ai dati della tabella, è necessario ancora una volta ciclo e analizzare il Elementi HTML.</span><span class="sxs-lookup"><span data-stu-id="8297e-201">It would take significant effort to achieve this ourselves using jQuery or direct DOM manipulation, and even then if we then wanted to display aggregate results (such as a win-loss record) based on the table's data, we would need to once more loop through it and parse the HTML elements.</span></span>  <span data-ttu-id="8297e-202">Con Knockout, la visualizzazione del record win perdita è piuttosto semplice.</span><span class="sxs-lookup"><span data-stu-id="8297e-202">With Knockout, displaying the win-loss record is trivial.</span></span> <span data-ttu-id="8297e-203">È possibile eseguire i calcoli all'interno di ViewModel stesso e quindi visualizzarla con un'associazione di testo semplice e un `<span>`.</span><span class="sxs-lookup"><span data-stu-id="8297e-203">We can perform the calculations within the ViewModel itself, and then display it with a simple text binding and a `<span>`.</span></span>

<span data-ttu-id="8297e-204">Per compilare la stringa di record win perdita, è possibile usare un observable calcolata.</span><span class="sxs-lookup"><span data-stu-id="8297e-204">To build the win-loss record string, we can use a computed observable.</span></span> <span data-ttu-id="8297e-205">Si noti che i riferimenti alle proprietà observable all'interno di ViewModel devono essere chiamate di funzione, in caso contrario non si recupererà il valore di observable (ad esempio `gameResults()` non `gameResults` nel codice visualizzato):</span><span class="sxs-lookup"><span data-stu-id="8297e-205">Note that references to observable properties within the ViewModel must be function calls, otherwise they will not retrieve the value of the observable (i.e. `gameResults()` not `gameResults` in the code shown):</span></span>

```javascript
self.displayRecord = ko.computed(function () {
  var wins = self.gameResults().filter(function (value) { return value.result() == "Win"; }).length;
  var losses = self.gameResults().filter(function (value) { return value.result() == "Loss"; }).length;
  var ties = self.gameResults().filter(function (value) { return value.result() == "Tie"; }).length;
  return wins + " - " + losses + " - " + ties;
}, this);
```

<span data-ttu-id="8297e-206">Associare questa funzione a un intervallo all'interno di `<h1>` elemento nella parte superiore della pagina:</span><span class="sxs-lookup"><span data-stu-id="8297e-206">Bind this function to a span within the `<h1>` element at the top of the page:</span></span>

```html
<h1>Record <span data-bind="text: displayRecord"></span></h1>
```

<span data-ttu-id="8297e-207">il risultato:</span><span class="sxs-lookup"><span data-stu-id="8297e-207">The result:</span></span>

![Perdita di trattative concluse](knockout/_static/record-winloss-screenshot.png)

<span data-ttu-id="8297e-209">Aggiunge righe o si modifica l'elemento selezionato nella colonna di risultati di qualsiasi riga verrà aggiornati al record visualizzato nella parte superiore della finestra.</span><span class="sxs-lookup"><span data-stu-id="8297e-209">Adding rows or modifying the selected element in any row's Result column will update the record shown at the top of the window.</span></span>

<span data-ttu-id="8297e-210">Oltre l'associazione a valori, è anche possibile usare qualsiasi espressione JavaScript validi all'interno di un'associazione.</span><span class="sxs-lookup"><span data-stu-id="8297e-210">In addition to binding to values, you can also use almost any legal JavaScript expression within a binding.</span></span> <span data-ttu-id="8297e-211">Ad esempio, se un elemento dell'interfaccia utente dovrebbe essere visualizzato solo in determinate condizioni, ad esempio quando un valore supera una determinata soglia, è possibile specificare questo logicamente all'interno dell'espressione di associazione:</span><span class="sxs-lookup"><span data-stu-id="8297e-211">For example, if a UI element should only appear under certain conditions, such as when a value exceeds a certain threshold, you can specify this logically within the binding expression:</span></span>

```html
<div data-bind="visible: customerValue > 100"></div>
```

<span data-ttu-id="8297e-212">Questo `<div>` saranno visibili solo quando il customerValue maggiori di 100.</span><span class="sxs-lookup"><span data-stu-id="8297e-212">This `<div>` will only be visible when the customerValue is over 100.</span></span>

## <a name="templates"></a><span data-ttu-id="8297e-213">Modelli</span><span class="sxs-lookup"><span data-stu-id="8297e-213">Templates</span></span>

<span data-ttu-id="8297e-214">Knockout dispone di supporto per i modelli, che è possibile separare facilmente l'interfaccia utente da un comportamento di o caricare in modo incrementale gli elementi dell'interfaccia utente in un'applicazione di grandi dimensioni su richiesta.</span><span class="sxs-lookup"><span data-stu-id="8297e-214">Knockout has support for templates, so that you can easily separate your UI from your behavior, or incrementally load UI elements into a large application on demand.</span></span> <span data-ttu-id="8297e-215">È possibile aggiornare l'esempio precedente per rendere il proprio modello di ogni riga semplicemente estrazione HTML out in un modello e specificando il modello in base al nome nella chiamata di un'associazione dati in `<tbody>`.</span><span class="sxs-lookup"><span data-stu-id="8297e-215">We can update our previous example to make each row its own template by simply pulling the HTML out into a template and specifying the template by name in the data-bind call on `<tbody>`.</span></span>

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

<span data-ttu-id="8297e-216">Knockout supporta anche altri motori di modelli, ad esempio la libreria jQuery.tmpl e motore di Underscore.js modello.</span><span class="sxs-lookup"><span data-stu-id="8297e-216">Knockout also supports other templating engines, such as the jQuery.tmpl library and Underscore.js's templating engine.</span></span>

## <a name="components"></a><span data-ttu-id="8297e-217">Componenti</span><span class="sxs-lookup"><span data-stu-id="8297e-217">Components</span></span>

<span data-ttu-id="8297e-218">Componenti consentono di organizzare e riutilizzare il codice dell'interfaccia utente, in genere insieme ai dati ViewModel da cui dipende il codice dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="8297e-218">Components allow you to organize and reuse UI code, usually along with the ViewModel data on which the UI code depends.</span></span> <span data-ttu-id="8297e-219">Per creare un componente, è sufficiente specificare il modello e il relativo viewModel e assegnargli un nome.</span><span class="sxs-lookup"><span data-stu-id="8297e-219">To create a component, you simply need to specify its template and its viewModel, and give it a name.</span></span> <span data-ttu-id="8297e-220">Questa operazione viene effettuata chiamando `ko.components.register()`.</span><span class="sxs-lookup"><span data-stu-id="8297e-220">This is done by calling `ko.components.register()`.</span></span> <span data-ttu-id="8297e-221">Inoltre, per definire i modelli e viewmodel inline, possono essere caricati da file esterni mediante una libreria come *require.js*, con conseguente codice molto pulito ed efficiente.</span><span class="sxs-lookup"><span data-stu-id="8297e-221">In addition to defining the templates and viewmodel inline, they can be loaded from external files using a library like *require.js*, resulting in very clean and efficient code.</span></span>

## <a name="communicating-with-apis"></a><span data-ttu-id="8297e-222">La comunicazione con le API</span><span class="sxs-lookup"><span data-stu-id="8297e-222">Communicating with APIs</span></span>

<span data-ttu-id="8297e-223">Knockout lavorare con i dati in formato JSON.</span><span class="sxs-lookup"><span data-stu-id="8297e-223">Knockout can work with any data in JSON format.</span></span> <span data-ttu-id="8297e-224">È un modo comune per recuperare e salvare i dati usando Knockout con jQuery, che supporta il `$.getJSON()` funzione per recuperare i dati e `$.post()` metodo per inviare dati dal browser a un endpoint dell'API.</span><span class="sxs-lookup"><span data-stu-id="8297e-224">A common way to retrieve and save data using Knockout is with jQuery, which supports the `$.getJSON()` function to retrieve data, and the `$.post()` method to send data from the browser to an API endpoint.</span></span> <span data-ttu-id="8297e-225">Naturalmente, se si preferisce un modo diverso per inviare e ricevere i dati JSON, Knockout funzionerà con esso anche.</span><span class="sxs-lookup"><span data-stu-id="8297e-225">Of course, if you prefer a different way to send and receive JSON data, Knockout will work with it as well.</span></span>

## <a name="summary"></a><span data-ttu-id="8297e-226">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="8297e-226">Summary</span></span>

<span data-ttu-id="8297e-227">Knockout fornisce un modo semplice ed elegante per associare lo stato corrente dell'applicazione client, definito in un elemento ViewModel di elementi dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="8297e-227">Knockout provides a simple, elegant way to bind UI elements to the current state of the client application, defined in a ViewModel.</span></span> <span data-ttu-id="8297e-228">La sintassi di associazione del Knockout utilizza l'attributo data-bind, applicato agli elementi HTML che devono essere elaborati.</span><span class="sxs-lookup"><span data-stu-id="8297e-228">Knockout's binding syntax uses the data-bind attribute, applied to HTML elements that are to be processed.</span></span> <span data-ttu-id="8297e-229">È in grado di eseguire il rendering in modo efficiente e aggiornare i set di dati di grandi dimensioni grazie al rilevamento di elementi dell'interfaccia utente Knockout e solo l'elaborazione delle modifiche per interessati elementi.</span><span class="sxs-lookup"><span data-stu-id="8297e-229">Knockout is able to efficiently render and update large data sets by tracking UI elements and only processing changes to affected elements.</span></span> <span data-ttu-id="8297e-230">Applicazioni di grandi dimensioni possono suddividere la logica dell'interfaccia utente mediante modelli e componenti, che possono essere caricati su richiesta da file esterni.</span><span class="sxs-lookup"><span data-stu-id="8297e-230">Large applications can break up UI logic using templates and components, which can be loaded on demand from external files.</span></span> <span data-ttu-id="8297e-231">Attualmente versione 3, Knockout è una libreria JavaScript stabile che può migliorare le applicazioni web che richiedono di interattività rich client.</span><span class="sxs-lookup"><span data-stu-id="8297e-231">Currently version 3, Knockout is a stable JavaScript library that can improve web applications that require rich client interactivity.</span></span>
