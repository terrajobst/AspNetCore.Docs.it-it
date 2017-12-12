---
title: Utilizzo di AngularJS per applicazioni a pagina singola (stabilimenti termali)
author: rick-anderson
description: Informazioni su come creare un'applicazione ASP.NET SPA stile con AngularJS
keywords: ASP.NET Core, AngularJS, SPA
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4b30576b-2718-4c39-9253-a59966747893
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/angular
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ccdf1625cdaf2400780500ac5ab86f41537964a9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="using-angularjs-for-single-page-applications-spas-with-aspnet-core"></a>Utilizzo di AngularJS per applicazioni a pagina singola (stabilimenti termali) con ASP.NET Core


Da [Venkata Koppaka](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/) e [Scott Addie](https://scottaddie.com)

In questo articolo si apprenderà come compilare un'applicazione ASP.NET SPA stile con AngularJS.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-angularjs"></a>Che cos'è AngularJS?

[AngularJS](https://angularjs.org/) è un moderno framework JavaScript da Google comunemente utilizzato per l'utilizzo con applicazioni a pagina singola (stabilimenti termali). AngularJS è open source con licenza MIT e può essere seguito sullo sviluppo di AngularJS [relativo repository GitHub](https://github.com/angular/angular.js). La raccolta viene denominata angolare perché HTML Usa parentesi angolare a forma di.

AngularJS non è una libreria di manipolazione di DOM come jQuery, ma usa un sottoinsieme di jQuery chiamato jQLite. AngularJS è basato principalmente su attributi dichiarativi di HTML che è possibile aggiungere i tag HTML. È possibile provare AngularJS nel browser utilizzando il [sito Web dell'istituto di istruzione di codice](https://www.codeschool.com/courses/shaping-up-with-angularjs) o [sito Web W3Schools](https://www.w3schools.com/angular/).

Questo articolo è incentrato su AngularJS con alcune note su diretto in cui angolare.

## <a name="getting-started"></a>Per iniziare

Per iniziare a utilizzare AngularJS nell'applicazione ASP.NET, è necessario installarlo come parte del progetto o farvi riferimento da una rete di distribuzione di contenuti (CDN).

### <a name="installation"></a>Installazione

Esistono diversi modi per aggiungere AngularJS all'applicazione. Se si inizia una nuova applicazione web ASP.NET Core in Visual Studio, è possibile aggiungere AngularJS utilizzando l'elemento predefinito [Bower](bower.md) supportano. Aprire *bower. JSON*e aggiungere una voce per il `dependencies` proprietà:

<a name="angular-bower-json"></a>

[!code-json[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/bower.json?highlight=9)]

Al momento del salvataggio il *bower. JSON* angolare dei file di installazione del progetto *wwwroot/lib* cartella. Inoltre, verrà elencato all'interno di `Dependencies/Bower` cartella. Vedere la schermata seguente.

![AngularJS progetto in Esplora soluzioni](angular/_static/angular-solution-explorer.png)

Successivamente, aggiungere un `<script>` riferimento a fondo il `<body>` sezione della pagina HTML o *layout. cshtml* file, come illustrato di seguito:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=4&range=48-52)]

È consigliabile che le applicazioni di produzione usano CDN per le librerie comuni, ad esempio AngularJS. È possibile fare riferimento a AngularJS da una delle diverse reti CDN, ad esempio questa:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=10&range=53-67)]

Una volta ottenuto un riferimento di *angular.js* file di script, si è pronti per iniziare a usare AngularJS nelle pagine web.

## <a name="key-components"></a>Componenti chiave

AngularJS include un numero di componenti principali, ad esempio *direttive*, *modelli*, *controlli Repeater*, *moduli*,  *controller*, *componenti*, *router componente* e altro ancora. Esaminiamo questi componenti interagiscono per aggiungere un comportamento alle pagine web.

### <a name="directives"></a>Direttive

Usa AngularJS [direttive](https://docs.angularjs.org/guide/directive) estendere HTML con elementi e attributi personalizzati. Direttive AngularJS sono definite tramite `data-ng-*` o `ng-*` prefissi (`ng` è breve angolare). Esistono due tipi di direttive AngularJS:

   1. **Direttive primitive**: questi sono già definiti dal team di angolare e fanno parte del framework AngularJS.

   2. **Direttive personalizzate**: si tratta di direttive personalizzate che è possibile definire.

Una delle direttive di primitivi utilizzate in tutte le applicazioni di AngularJS è il `ng-app` direttiva, che avvia l'applicazione di AngularJS. Questa direttiva può essere applicata al `<body>` tag o a un elemento figlio del corpo. Di seguito viene illustrato un esempio in azione. Supponendo che si trova in un progetto ASP.NET, è possibile aggiungere un file HTML per il `wwwroot` cartella, oppure aggiungere una nuova azione di controller e una visualizzazione associata. In questo caso, ho aggiunto un nuovo `Directives` metodo di azione da `HomeController.cs`. La visualizzazione associata è illustrata di seguito:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Directives.cshtml?highlight=5,7)]

Per mantenere questi esempi indipendenti una da altra, non si utilizza il file di layout condivise. È possibile vedere che è decorata con tag body il `ng-app` direttiva per indicare questa pagina è un'applicazione di AngularJS. Il `{{2+2}}` è un'espressione di associazione di dati angolare ulteriori informazioni in proposito. Ecco il risultato, se si esegue l'applicazione:

![Direttiva Angular semplice](angular/_static/simple-directive.png)

Altre direttive primitivi in AngularJS includono:

`ng-controller`Determina quale controller JavaScript è associato alla visualizzazione.

`ng-model`Determina il modello a cui sono associati i valori delle proprietà dell'elemento HTML.

`ng-init`Utilizzato per inizializzare i dati dell'applicazione sotto forma di un'espressione per l'ambito corrente.

`ng-if`Rimuove o ricrea l'elemento HTML specificato nel DOM in base a truthiness dell'espressione fornita.

`ng-repeat`Ripete un blocco di HTML specifico su un set di dati.

`ng-show`Mostra o nasconde l'elemento HTML specificato in base all'espressione fornita.

Per un elenco completo di tutte le direttive primitivi supportati in AngularJS, consultare il [una sezione di documentazione direttiva nel sito Web di documentazione AngularJS](https://docs.angularjs.org/api/ng/directive).

### <a name="data-binding"></a>Associazione dati

Fornisce AngularJS [associazione dati](https://docs.angularjs.org/guide/databinding) supporto out-of-the-box utilizzando il `ng-bind` direttiva o un data binding, ad esempio la sintassi dell'espressione `{{expression}}`. AngularJS supporta l'associazione bidirezionale dei dati in cui i dati da un modello viene mantenuti in sincronizzazione con un modello di visualizzazione in qualsiasi momento. Tutte le modifiche alla visualizzazione vengono automaticamente applicate al modello. Analogamente, tutte le modifiche nel modello vengono riflesse nella visualizzazione.

Creare un file HTML o un'azione del controller con una vista associata denominata `Databinding`. Nella visualizzazione, includono i seguenti:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Databinding.cshtml?highlight=8,9,10)]

Si noti che è possibile visualizzare i valori del modello utilizzando l'associazione di direttive o dati (`ng-bind`). Nella pagina risultante dovrebbe essere simile al seguente:

![Associazione dati semplice](angular/_static/simple-databinding.png)

### <a name="templates"></a>Modelli

[Modelli](https://docs.angularjs.org/guide/templates) in AngularJS pagine HTML semplice decorate con direttive AngularJS e gli elementi. Un modello AngularJS è una combinazione di direttive, espressioni, filtri e i controlli che consentono di combinare in HTML per la visualizzazione form.

Aggiungere un'altra visualizzazione per mostrare i modelli e aggiungere quanto segue:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Templates.cshtml?highlight=8,9,10)]

Il modello dispone di direttive AngularJS come `ng-app`, `ng-init`, `ng-model` e sintassi di espressione di associazione dati per associare il `personName` proprietà. In esecuzione nel browser, la visualizzazione è simile nella schermata seguente:

![Esempio di modelli semplice 1](angular/_static/simple-templates-1.png)

Se si modifica il nome digitando nel campo di input, verrà visualizzato il testo accanto al campo di input in modo dinamico aggiornamento, che mostra l'associazione dati bidirezionale angolare in azione.

![Esempio di modelli semplice 2](angular/_static/simple-templates-2.png)

### <a name="expressions"></a>Espressioni

[Espressioni](https://docs.angularjs.org/guide/expression) in AngularJS sono frammenti di codice JavaScript che vengono scritte all'interno di `{{ expression }}` sintassi. L'associazione dei dati da queste espressioni HTML nello stesso modo `ng-bind` direttive. La differenza principale tra AngularJS espressioni ed espressioni regolari di JavaScript è tale AngularJS espressioni vengono valutate in base il `$scope` oggetto AngularJS.

Le espressioni di AngularJS l'esempio riportato di seguito binding `personName` e un semplice JavaScript calcolati espressione:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Expressions.cshtml?highlight=8,9,10)]

L'esempio in esecuzione nel browser viene visualizzata la `personName` dati e i risultati del calcolo:

![Espressioni semplici](angular/_static/simple-expressions.png)

### <a name="repeaters"></a>Controlli Repeater

Ripetizione di AngularJS viene eseguita tramite una direttiva primitiva chiamata `ng-repeat`. Il `ng-repeat` direttiva ripete un elemento HTML specificato in una vista per la durata di una matrice di dati ripetuto. Controlli Repeater in AngularJS possibile ripetere su una matrice di stringhe o oggetti. Di seguito è riportato un esempio di utilizzo di ripetizione di una matrice di stringhe:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters.cshtml?highlight=8,10,11)]

Il [repeat (direttiva)](https://docs.angularjs.org/api/ng/directive/ngRepeat) genera una serie di elementi dell'elenco in un elenco non ordinato, come si può notare in strumenti di sviluppo illustrati in questo screenshot:

![Esempio di Repeater](angular/_static/repeater.png)

Di seguito è riportato un esempio che si ripete su una matrice di oggetti. Il `ng-init` direttiva stabilisce un `names` matrice, in cui ogni elemento è un oggetto contenente prima e il cognome. Il `ng-repeat` assegnazione, `name in names`, restituisce un elemento di elenco per ogni elemento della matrice.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters2.cshtml?highlight=8,9,10,11,13,14)]

In questo caso l'output è uguale a quello dell'esempio precedente.

Angolare fornisce alcune direttive aggiuntive che consentono di fornire un comportamento in base a dove il ciclo è in esecuzione.

`$index`

Utilizzare `$index` nel `ng-repeat` ciclo per determinare quale indice posizionare il ciclo attualmente è in.

`$even` e `$odd`

Utilizzare `$even` nel `ng-repeat` ciclo per determinare se l'indice nel ciclo corrente è una riga anche indicizzata. Analogamente, utilizzare `$odd` per determinare se l'indice corrente è una riga indicizzata dispari.

`$first` e `$last`

Utilizzare `$first` nel `ng-repeat` ciclo per determinare se l'indice nel ciclo corrente è la prima riga. Analogamente, utilizzare `$last` per determinare se l'indice corrente è l'ultima riga.

Di seguito è riportato un esempio che illustra `$index`, `$even`, `$odd`, `$first`, e `$last` in azione:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters3.cshtml?highlight=14,15,16,17,18)]

Di seguito è riportato l'output risultante:

![Esempio Ripetitore 2](angular/_static/repeaters2.png)

### <a name="scope"></a>$scope

`$scope`è un oggetto JavaScript che fungono da collante tra la visualizzazione (modello) e il controller (come illustrato di seguito). Un modello di visualizzazione in AngularJS viene a conoscenza solo i valori collegati al `$scope` oggetto nel controller.

> [!NOTE]
> Nel mondo, MVVM il `$scope` oggetto AngularJS è spesso definito come il ViewModel. Il team di AngularJS si intende il `$scope` oggetto come modello di dati. [Altre informazioni sugli ambiti di AngularJS](https://docs.angularjs.org/guide/scope).

Di seguito è riportato un esempio semplice che illustra come impostare le proprietà delle `$scope` all'interno di un file JavaScript separato, *scope.js*:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/scope.js?highlight=2,3)]

Osservare il `$scope` parametro passato per il controller nella riga 2. Questo oggetto è ciò che la vista viene a conoscenza. Nella riga 3, si sta impostando una proprietà denominata "name" a "Maria Jane".

Cosa accade quando una determinata proprietà non viene trovata la vista? La vista definita di seguito fa riferimento alla proprietà "name" e "age":

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Scope.cshtml?highlight=9,10,14)]

Notare che alla riga 9 che viene richiesto di angolare per visualizzare la proprietà "name" utilizzando la sintassi dell'espressione. Riga 10 fa quindi riferimento a "age", una proprietà che non esiste. L'esempio mostra il nome impostato su "Mary Jane" e non per età. Proprietà mancanti vengono ignorate.

![Esempio di ambito](angular/_static/scope.png)

### <a name="modules"></a>Moduli

Oggetto [modulo](https://docs.angularjs.org/guide/module) in AngularJS è una raccolta di controller di servizi, le direttive, e così via. Il `angular.module()` chiamata di funzione viene utilizzata per creare, registrare e recuperare i moduli AngularJS. Tutti i moduli, inclusi quelli forniti dal team di AngularJS e librerie di terze parti, devono essere registrati tramite il `angular.module()` (funzione).

Di seguito è riportato un frammento di codice che illustra come creare un nuovo modulo in AngularJS. Il primo parametro è il nome del modulo. Il secondo parametro definisce le dipendenze in altri moduli. Più avanti in questo articolo è mostrerà come passare queste dipendenze per un `angular.module()` chiamata al metodo.

```javascript
var personApp = angular.module('personApp', []);
```

Utilizzare il `ng-app` direttiva per rappresentare un modulo AngularJS nella pagina. Per usare un modulo, assegnare il nome del modulo, `personApp` in questo esempio, per il `ng-app` direttiva di modelli.

```html
<body ng-app="personApp">
```

### <a name="controllers"></a>Controller

[Controller](https://docs.angularjs.org/guide/controller) in AngularJS sono il primo punto di ingresso per il codice. Il `<module name>.controller()` chiamata di funzione viene utilizzata per creare e registrare i controller in AngularJS. Il `ng-controller` direttiva viene utilizzata per rappresentare un controller AngularJS nella pagina HTML. Il ruolo del controller nel angolare consiste nell'impostare lo stato e il comportamento del modello di dati (`$scope`). Controller non deve essere utilizzati per modificare direttamente il DOM.

Di seguito è riportato un frammento di codice che registra un nuovo controller. Il `personApp` variabile nel frammento di fa riferimento a un modulo angolare, definito nella riga 2.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/controllers.js?highlight=2,5)]

La vista che utilizza il `ng-controller` direttiva assegna il nome del controller:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Controllers.cshtml?highlight=8,14)]

Nella pagina viene visualizzato "Maria" e "Jane" che corrispondono al `firstName` e `lastName` proprietà collegata la `$scope` oggetto:

![Esempio di controller](angular/_static/controllers.png)

### <a name="components"></a>Componenti

[Componenti](https://docs.angularjs.org/guide/component) in angolare 1.5 consentire per l'incapsulamento e la capacità di creazione di singoli elementi HTML. In 1.4.x angolare è possibile ottenere la stessa funzionalità utilizzando il metodo .directive().

Tramite il metodo .component(), lo sviluppo è stato semplificato ottenere la funzionalità della direttiva e il controller. Altri vantaggi includono; isolamento dell'ambito, le procedure consigliate sono intrinseche e la migrazione a 2 angolare diventa un'attività semplice. Il `<module name>.component()` chiamata di funzione viene utilizzata per creare e registrare i componenti in AngularJS.

Di seguito è riportato un frammento di codice che registra un nuovo componente. Il `personApp` variabile nel frammento di fa riferimento a un modulo angolare, definito nella riga 2.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/components.js?highlight=2,5,13)]

La vista in cui viene visualizzato l'elemento HTML personalizzato.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Components.cshtml?highlight=8)]

Il modello associato utilizzato dal componente:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personcomponent.html?highlight=2,3)]

Nella pagina viene visualizzato "Aftab" e "Ansari" che corrispondono al `firstName` e `lastName` proprietà collegata la `vm` oggetto:

![Esempio di componenti](angular/_static/components.png)

### <a name="services"></a>Servizi

[Servizi](https://docs.angularjs.org/guide/services) in AngularJS sono in genere utilizzati per il codice condiviso che viene estratto in un file che può essere utilizzato per tutta la durata di un'applicazione angolare. Servizi in modo differito creazione di istanze, vale a dire che non sarà presente un'istanza di un servizio a meno che non viene utilizzato un componente che dipende dal servizio. Factory sono un esempio di servizio utilizzato nelle applicazioni di AngularJS. Factory vengono create utilizzando il `myApp.factory()` chiamata di funzione, in cui `myApp` è il modulo.

Di seguito è riportato un esempio che illustra come usare una factory di AngularJS:

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/simpleFactory.js?highlight=1)]

Per chiamare questa factory dal controller, passare `personFactory` come parametro per il `controller` funzione:

```javascript
personApp.controller('personController', function($scope,personFactory) {
  $scope.name = personFactory.getName();
});
```

### <a name="using-services-to-talk-to-a-rest-endpoint"></a>Utilizzo dei servizi per comunicare con un endpoint REST

Di seguito è riportato un esempio end-to-end utilizzando servizi in AngularJS per interagire con un endpoint API Web di ASP.NET Core. L'esempio ottiene i dati dall'API Web e visualizza i dati in un modello di visualizzazione. Iniziamo con la vista prima:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Index.cshtml?highlight=5,8,10,17,18,19)]

In questa vista, si dispone di un modulo angolare chiamato `PersonsApp` e un controller denominato `personController`. Si sta usando `ng-repeat` per scorrere l'elenco di persone. Ci stiamo che fanno riferimento a tre file JavaScript personalizzati nelle righe 17-19.

Il *personApp.js* file viene utilizzato per registrare il `PersonsApp` modulo; e la sintassi è simile agli esempi precedenti. Si sta usando il `angular.module` funzione per creare una nuova istanza del modulo che verrà utilizzato con.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personApp.js?highlight=3)]

Esaminiamo un *personFactory.js*riportato di seguito. Qui viene chiamato il modulo `factory` metodo per creare una factory. La riga 12 mostra l'angolare incorporato `$http` servizio recupero delle informazioni di utenti da un servizio web.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personFactory.js?highlight=6,7,12)]

In *personController.js*, venga chiamato il modulo `controller` metodo per creare il controller. Il `$scope` dell'oggetto `people` proprietà viene assegnato ai dati restituiti da personFactory (riga 13).

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personController.js?highlight=6,7,13)]

Diamo un'occhiata veloce l'API Web e il modello sottostante. Il `Person` modello è un POCO (Plain oggetto CLR precedente) con `Id`, `FirstName`, e `LastName` proprietà:

[!code-csharp[Main](angular/sample/AngularJSSample/src/AngularJSSample/Models/Person.cs)]

Il `Person` controller restituisce un elenco in formato JSON di `Person` oggetti:

[!code-csharp[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Controllers/Api/PersonController.cs?highlight=9,10,19)]

Di seguito viene illustrato il funzionamento dell'applicazione:

![Controller visualizzare altri risultati](angular/_static/rest-bound.png)

È possibile [consente di visualizzare la struttura dell'applicazione su GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).

> [!NOTE]
> Per ulteriori informazioni su come strutturare applicazioni AngularJS, vedere [Guida della John Papa di stile Angular](https://github.com/johnpapa/angular-styleguide)

&nbsp;

> [!NOTE]
> Per creare un modulo AngularJS, controller, factory, file direttiva e visualizzare facilmente, assicurarsi di estrarre Sayed Hashimi [SideWaffle pack dei modelli di Visual Studio](http://sidewaffle.com/). Hashimi sayed è un Senior Program Manager del Team di Visual Studio Web in Microsoft e i modelli di SideWaffle sono considerati lo standard. Al momento della redazione del presente documento, è disponibile per Visual Studio 2012, 2013 e 2015 SideWaffle.

### <a name="routing-and-multiple-views"></a>Routing e visualizzazioni multiple

AngularJS dispone di un provider di route predefinite per la navigazione di base di SPA (applicazione a pagina singola). Per lavorare con il routing di AngularJS, è necessario aggiungere il `angular-route` libreria tramite Bower. È possibile visualizzare nel [bower. JSON](#angular-bower-json) file a cui fa riferimento all'inizio di questo articolo che si sta già farvi riferimento nel progetto.

Dopo aver installato il pacchetto, aggiungere il riferimento allo script (*angolare route.js*) alla propria visualizzazione.

Ora prendiamo l'App di persona che si sta compilando e aggiungere la navigazione a esso. In primo luogo, si creerà una copia dell'app mediante la creazione di un nuovo `PeopleController` azione denominata `Spa` e un oggetto corrispondente `Spa.cshtml` visualizzazione copiando la vista cshtml il `People` cartella. Aggiungere un riferimento allo script `angular-route` (vedere la riga 11). Aggiungere anche un `div` contrassegnato con il `ng-view` (direttiva) (vedere la riga 6) come segnaposto per inserire le visualizzazioni. Occorre disporre di diversi altri *. js* file a cui fanno riferimento alle righe 13-16.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Spa.cshtml?highlight=6,11,12,13,14,15,16)]

Esaminiamo un *personModule.js* file per vedere come si sta creando un'istanza il modulo con il routing. Viene passato `ngRoute` come libreria nel modulo. Questo modulo gestisce routing nell'applicazione.

[!code-javascript[Main](angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personModule.js)]

Il *personRoutes.js* file, di seguito, definisce le route in base al provider di route. Definiscono la navigazione pronunciando in modo efficace, quando un URL con righe 4-7 `/persons` è richiesto, utilizzare un modello denominato `partials/personlist` affrontando `personListController`. Le righe da 8 a 11 indicano una pagina di dettaglio con un parametro di route di `personId`. Se l'URL non corrisponde a uno dei modelli, angolare per impostazione predefinita il `/persons` visualizzazione.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personRoutes.js?highlight=4,5,6,7,8,9,10,11,13)]

Il `personlist.html` file è una visualizzazione parziale contenente solo il codice HTML necessario per la visualizzazione elenco.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personlist.html?highlight=3)]

Il controller viene definito mediante il modulo `controller` funzionare in *personListController.js*.

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personListController.js?highlight=1)]

Se si esegue l'applicazione e passare il `people/spa#/persons` URL, verrà spiegato:

![Visualizzazione elenco di persone](angular/_static/spa-persons.png)

Se si passa a una pagina di dettaglio, ad esempio `people/spa#/persons/2`, vedremo della visualizzazione parziale di dettaglio:

![Visualizzazione di dettagli utente](angular/_static/spa-persons-2.png)

È possibile visualizzare il codice sorgente completo e tutti i file non visualizzati in questo articolo [GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample).

### <a name="event-handlers"></a>Gestori eventi

Esistono un numero di direttive di AngularJS che aggiungono funzionalità di gestione degli eventi per gli elementi di input in DOM. l'HTML Di seguito è un elenco di eventi che vengono compilati in AngularJS.

   * `ng-click`

   * `ng-dbl-click`

   * `ng-mousedown`

   * `ng-mouseup`

   * `ng-mouseenter`

   * `ng-mouseleave`

   * `ng-mousemove`

   * `ng-keydown`

   * `ng-keyup`

   * `ng-keypress`

   * `ng-change`

> [!NOTE]
> È possibile aggiungere i propri gestori di eventi utilizzando il [direttive personalizzate funzionalità in AngularJS](https://docs.angularjs.org/guide/directive).

Vengono descritte le modalità di `ng-click` è collegato l'evento. Creare un nuovo file JavaScript denominato *eventHandlerController.js*e aggiungere quanto segue:

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/eventHandlerController.js?highlight=5,6,7)]

Si noti il nuovo `sayName` funzionare in `eventHandlerController` nella riga 5 riportato sopra. Esegue tutti il metodo per l'ora viene visualizzato un avviso di JavaScript per l'utente con un messaggio di benvenuto.

La vista seguente associa una funzione di controller per un evento di AngularJS. Riga 9 dispone di un pulsante su cui il `ng-click` direttiva Angular è stata applicata. Chiama il nostro `sayName` funzione, a cui è associato il `$scope` oggetto passato a questa vista.

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Events.cshtml?highlight=9)]

L'esempio dimostra che il controller `sayName` funzione viene chiamata automaticamente quando si fa clic sul pulsante.

![Click (evento)](angular/_static/events.png)

Per ulteriori informazioni sulle direttive di gestore eventi predefinito AngularJS, assicurarsi di intestazione per il [sito Web di documentazione](https://docs.angularjs.org/api/ng/directive/ngClick) di AngularJS.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Documenti Angular](https://docs.angularjs.org)

* [Info 2 Angular](https://angular.io/)
