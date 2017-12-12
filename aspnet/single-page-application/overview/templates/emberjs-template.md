---
uid: single-page-application/overview/templates/emberjs-template
title: Modello EmberJS | Documenti Microsoft
author: xqiu
description: Modello EmberJS
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1fb7633aee288be648d4f9681b43c8911b7dbab9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="emberjs-template"></a>Modello EmberJS
====================
da [Xinyang Qiu](https://github.com/xqiu)

> Il modello MVC EmberJS scritto da Nathan Totten, Thiago Santos e Xinyang Qiu.
> 
> [Scaricare il modello MVC EmberJS](https://go.microsoft.com/fwlink/?LinkId=282647)


Il modello di SPA EmberJS è progettato per iniziare a creare rapidamente applicazioni web sul lato client interattive utilizzando EmberJS.

"L'applicazione a pagina singola" (SPA) è il termine generale per un'applicazione web che carica una singola pagina HTML e quindi aggiorna la pagina in modo dinamico, invece di caricare nuove pagine. Dopo il caricamento della pagina iniziale, l'autenticazione SPA comunica con il server tramite le richieste AJAX.

![](emberjs-template/_static/image1.png)

AJAX è un concetto nuovo, ma esistono oggi Framework JavaScript che rendono più semplice compilare e gestire un'applicazione SPA sofisticata di grandi dimensioni. Inoltre, HTML5 e CSS3 sono rendendo più semplice creare interfacce utente avanzate.

Il modello di SPA EmberJS utilizza il [Ember](http://emberjs.com/) libreria JavaScript di gestire gli aggiornamenti a pagina da richieste AJAX. Ember.js utilizza l'associazione dati per sincronizzare la pagina con i dati più recenti. In questo modo, non è necessario scrivere codice che esamina i dati JSON e aggiorna il modello DOM. È invece possibile inserire attributi dichiarativi nel codice HTML che fornisce Ember.js modalità presentare i dati.

Sul lato server, il modello di EmberJS è quasi identica a quella di [modello Knockout.js SPA](../introduction/knockoutjs-template.md). Usa ASP.NET MVC per servire i documenti HTML e ASP.NET Web API per gestire le richieste AJAX dal client. Per ulteriori informazioni su questi aspetti del modello, vedere il [Knockout.js modello](../introduction/knockoutjs-template.md) documentazione. In questo argomento vengono illustrate le differenze tra il modello Knockout e il modello EmberJS.

## <a name="create-an-emberjs-spa-template-project"></a>Creare un progetto di modello SPA EmberJS

Scaricare e installare il modello fare clic sul pulsante Download. Si potrebbe essere necessario riavviare Visual Studio.

Nel **modelli** riquadro, selezionare **modelli installati** ed espandere il **Visual c#** nodo. In **Visual c#**selezionare **Web**. Nell'elenco dei modelli di progetto, selezionare **applicazione Web ASP.NET MVC 4**. Denominare il progetto e fare clic su **OK**.

![](emberjs-template/_static/image2.png)

Nel **nuovo progetto** procedura guidata, selezionare **Ember.js SPA progetto**.

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a>Cenni preliminari sui modelli SPA EmberJS

Il modello EmberJS utilizza una combinazione di jQuery, Ember.js, Handlebars.js per creare un'interfaccia utente interattiva uniforme.

Ember.js è una libreria JavaScript che utilizza un modello MVC sul lato client.

- Oggetto *modello*scritta nella lingua del modello, i manubri delle descrive l'interfaccia utente dell'applicazione. In modalità di rilascio, il [compilatore hl](https://github.com/Myslik/csharp-ember-handlebars) viene utilizzato per aggregare e compilare il modello hl.
- Oggetto *modello* archivia i dati dell'applicazione che ottiene dal server (elenchi di attività e attività).
- Oggetto *controller* archivia lo stato dell'applicazione. I controller di presentano i dati del modello per i modelli corrispondenti.
- Oggetto *vista* converte gli eventi primitivi dall'applicazione e le passa al controller.
- Oggetto *router* gestisce lo stato dell'applicazione, mantenere sincronizzati i modelli e gli URL.

Inoltre, la raccolta dati Ember può essere usata per sincronizzare oggetti JSON (ottenuti dal server tramite un'API RESTful) e i modelli di client.

Il modello di SPA EmberJS organizza gli script in otto livelli:

- webapi\_adapter.js, webapi\_serializer.js: estende la libreria Ember dati per l'uso con ASP.NET Web API.
- Scripts/helpers.js: Definisce nuovi helper i manubri delle Ember.
- Scripts/app.js: Crea l'app e configura l'adattatore e il serializzatore.
- Gli script o app o imodelli/\*. js: definisce i modelli.
- Gli script eapp/viste/\*. js: definisce le viste.
- Script/applicazione/controller/\*. js: definisce i controller.
- Script/applicazione/route, Scripts/app/router.js: Definisce le route.
- Modelli /\*.hbs: definisce i modelli i manubri delle.

Esaminiamo alcuni di questi script in modo più dettagliato.

## <a name="models"></a>Modelli

I modelli sono definiti nella cartella script o app o i modelli. Sono disponibili due file di modello: todoItem.js e todoList.js.

**TODO.Model.js** definisce i modelli sul lato client (browser) per gli elenchi di attività da eseguire. Esistono due classi di modello: todoItem e todoList. In Ember, i modelli sono sottoclassi di dominio Active Directory. Modello. Un modello può disporre di proprietà con attributi:

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

I modelli è possono definire relazioni con altri modelli:

[!code-css[Main](emberjs-template/samples/sample2.css)]

I modelli possono sono calcolati delle proprietà associate alle altre:

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

I modelli possono avere funzioni di osservatore, che vengono richiamate quando viene modificata una proprietà osservata:

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a>Visualizzazioni

Le viste sono definite nella cartella Script/applicazione/visualizzazioni. Una vista converte gli eventi dall'interfaccia utente dell'applicazione. Un gestore eventi può richiamare funzioni di controller o semplicemente chiamare direttamente il contesto dei dati.

Ad esempio, il codice seguente è da views/TodoItemEditView.js. Definisce la gestione degli eventi per un campo di testo di input.

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a>Controller

I controller sono definiti nella cartella Script/applicazione/controller. Per rappresentare un singolo modello, è possibile estendere `Ember.ObjectController`:

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

Un controller può anche rappresentare una raccolta di modelli tramite l'estensione `Ember.ArrayController`. Ad esempio, il TodoListController rappresenta una matrice di `todoList` oggetti. Il controller vengono ordinati ID elenco attività, in ordine decrescente:

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

Il controller definisce una funzione denominata `addTodoList`, che crea un nuovo elenco di attività e lo aggiunge alla matrice. Per visualizzare la modalità con cui questa funzione viene chiamata, aprire il file di modello denominato todoListTemplate.html, nella cartella dei modelli. Il seguente codice di modello viene associato un pulsante per la `addTodoList` funzione:

[!code-html[Main](emberjs-template/samples/sample8.html)]

Il controller contiene anche un `error` proprietà che contiene un messaggio di errore. Di seguito è riportato il codice del modello per visualizzare il messaggio di errore (anche in todoListTemplate.html):

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a>Route

Router.js definisce le route e il modello predefinito da visualizzare, impostare lo stato dell'applicazione e corrisponde a URL di route:

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

Per l'override della funzione setupController TodoListRoute.js carica i dati per il TodoListRoute:

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

Ember utilizza convenzioni di denominazione per individuare gli URL, i nomi di route, controller e modelli. Per ulteriori informazioni, vedere [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) la documentazione di EmberJS.

## <a name="templates"></a>Modelli

La cartella dei modelli contiene quattro modelli:

- Application.hbs: il modello predefinito che viene eseguito il rendering quando l'applicazione viene avviata.
- About.hbs: il modello per la route "circa /".
- index.hbs: il modello per la radice route "/".
- todoList.hbs: il modello per la "/ todo" route.
- \_NavBar.hbs: il modello definisce il menu di navigazione.

Il modello di applicazione funziona come una pagina master. Contiene un'intestazione, un piè di pagina e "{{presa}}" per inserire gli altri modelli a seconda della route. Per ulteriori informazioni sui modelli di applicazione in Ember, vedere [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).

Il "/ todoList" modello contiene due espressioni del ciclo. Il ciclo esterno è `{{#each controller}}`e il ciclo è `{{#each todos}}`. Il codice seguente viene illustrato un oggetto incorporato `Ember.Checkbox` visualizzare, un oggetto personalizzato `App.TodoItemEditView`e un collegamento con un `deleteTodo` azione.

[!code-html[Main](emberjs-template/samples/sample12.html)]

Il `HtmlHelperExtensions` , definita in Controllers/HtmlHelperExensions.cs, definisce un helper funzione memorizzare nella cache e inserire modello file quando **debug** è impostato su **true** nel file Web. config. Questa funzione viene chiamata dal file di visualizzazione ASP.NET MVC definito in Views/Home/App.cshtml:

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

Chiamato senza argomenti, la funzione esegue il rendering di tutti i file di modello nella cartella dei modelli. È inoltre possibile specificare una sottocartella o un file di modello specifico.

Quando **debug** è **false** in Web. config, l'applicazione include l'elemento di raggruppamento "~/bundles/templates". Questo elemento di raggruppamento viene aggiunto in BundleConfig.cs, utilizzando la libreria del compilatore hl:

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
