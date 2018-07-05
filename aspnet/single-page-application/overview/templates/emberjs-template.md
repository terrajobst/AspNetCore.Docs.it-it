---
uid: single-page-application/overview/templates/emberjs-template
title: Modello EmberJS | Microsoft Docs
author: xqiu
description: Modello EmberJS
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1f5b005180fed15c51b36417cdcd6acc98123c9a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374166"
---
<a name="emberjs-template"></a>Modello EmberJS
====================
da [Xinyang Qiu](https://github.com/xqiu)

> Il modello MVC EmberJS viene scritto da Nathan Totten, Thiago Santos e Xinyang Qiu.
> 
> [Scaricare il modello MVC EmberJS](https://go.microsoft.com/fwlink/?LinkId=282647)


Il modello EmberJS SPA è progettato per iniziare a creare rapidamente App web lato client interattivo usando EmberJS.

"Applicazione a singola pagina" (SPA) è il termine generale per un'applicazione web che carica una singola pagina HTML, quindi aggiorna la pagina in modo dinamico, invece di caricare nuove pagine. Dopo il caricamento della pagina iniziale, l'applicazione a singola pagina comunica con il server tramite le richieste AJAX.

![](emberjs-template/_static/image1.png)

AJAX è nulla di nuovo, ma oggi esistono Framework JavaScript che rendono più semplice compilare e gestire un'applicazione SPA sofisticata di grandi dimensioni. Inoltre, HTML5 e CSS3 vengono semplificando la creazione di interfacce utente avanzate.

Il modello di applicazione a singola pagina EmberJS Usa il [Ember](http://emberjs.com/) libreria JavaScript per gestire gli aggiornamenti a pagina dalle richieste AJAX. Ember utilizza l'associazione dati per sincronizzare la pagina con i dati più recenti. In questo modo, non devi scrivere codice che illustra in modo dettagliato i dati JSON e aggiorna il DOM. È invece inserire attributi dichiarativi nel codice HTML che indicano ember come presentare i dati.

Sul lato server, il modello EmberJS è quasi identico per le [modello Knockout. js SPA](../introduction/knockoutjs-template.md). Usa ASP.NET MVC per rendere disponibili i documenti HTML e ASP.NET Web API per gestire le richieste AJAX dal client. Per altre informazioni su tali aspetti del modello, vedere la [modello KnockoutJS](../introduction/knockoutjs-template.md) documentazione. In questo argomento vengono illustrate le differenze tra il modello Knockout e il modello EmberJS.

## <a name="create-an-emberjs-spa-template-project"></a>Creare un progetto di modello EmberJS SPA

Scaricare e installare il modello facendo clic sul pulsante Download precedente. Si potrebbe essere necessario riavviare Visual Studio.

Nel **modelli** riquadro, selezionare **modelli installati** ed espandere le **Visual c#** nodo. Sotto **Visual c#**, selezionare **Web**. Nell'elenco dei modelli di progetto, selezionare **applicazione Web ASP.NET MVC 4**. Denominare il progetto e fare clic su **OK**.

![](emberjs-template/_static/image2.png)

Nel **nuovo progetto** procedura guidata, selezionare **progetto SPA ember**.

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a>Panoramica del modello di applicazione a singola pagina EmberJS

Il modello EmberJS Usa una combinazione di jQuery, ember, Handlebars.js per creare un'interfaccia utente semplice e interattiva.

Ember è una libreria JavaScript che usa un modello MVC lato client.

- Oggetto *modello*, scritto in un linguaggio di modelli Handlebars, descrive l'interfaccia utente dell'applicazione. In modalità di rilascio, il [compilatore Handlebars](https://github.com/Myslik/csharp-ember-handlebars) consente di aggregare e compilare il modello handlebars.
- Oggetto *modello* archivia i dati dell'applicazione ottenuto dal server (elenchi di attività e gli elementi ToDo).
- Oggetto *controller* archivia lo stato dell'applicazione. I controller di presentano i dati del modello per i modelli corrispondenti.
- Oggetto *vista* converte gli eventi primitivi dell'applicazione e le passa al controller.
- Oggetto *router* gestisce lo stato dell'applicazione, mantenendo sincronizzati gli URL e modelli.

Inoltre, la raccolta dati Ember utilizzabile per sincronizzare gli oggetti JSON (ottenuti dal server tramite un'API RESTful) e i modelli di client.

Il modello EmberJS SPA consente di organizzare gli script in otto livelli:

- webapi\_adapter.js, webapi\_serializer.js: estende la libreria Ember Data per lavorare con l'API Web ASP.NET.
- Scripts/helpers.js: Definisce nuovi helper Ember Handlebars.
- Scripts/app.js: Crea l'app e configura l'adattatore e il serializzatore.
- Gliscript/app/modelli/\*. js: definisce i modelli.
- Gliscript/app/views/\*. js: definisce le viste.
- Gliscript/app/controller/\*. js: definisce i controller.
- Gli script/app/routes, Scripts/app/router.js: Definisce le route.
- Modelli /\*.hbs: definisce i modelli handlebars.

Esaminiamo alcuni di questi script in modo più dettagliato.

## <a name="models"></a>Modelli

I modelli sono definiti nella cartella Modelli/app/script. Sono disponibili due file di modello: todoitem. js e todoList.js.

**TODO.Model.js** definisce i modelli lato client (browser) per gli elenchi di attività da eseguire. Esistono due classi di modello: todoItem e todoList. In Ember, i modelli sono sottoclassi di dominio Active Directory. Modello. Un modello può avere proprietà con attributi:

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

I modelli è possono definire le relazioni e altri modelli di:

[!code-css[Main](emberjs-template/samples/sample2.css)]

I modelli possono sono calcolati delle proprietà associate alle altre proprietà:

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

I modelli possono essere funzioni observer, che vengono richiamate quando una proprietà osservata venga modificata:

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a>Visualizzazioni

Le viste sono definite nella cartella script/app/views. Una visualizzazione converte gli eventi dall'interfaccia utente dell'applicazione. Un gestore eventi può richiamare funzioni di controller o semplicemente chiamare direttamente il contesto dei dati.

Ad esempio, il codice seguente è da views/TodoItemEditView.js. Definisce la gestione degli eventi per un campo di testo di input.

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a>Controller

I controller sono definiti nella cartella script/app/controllers. Per rappresentare un singolo modello, estendere `Ember.ObjectController`:

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

Un controller può anche rappresentare una raccolta di modelli tramite l'estensione `Ember.ArrayController`. Ad esempio, di TodoListController rappresenta una matrice di `todoList` oggetti. Il controller vengono ordinati dall'ID di todoList, in ordine decrescente:

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

Il controller definisce una funzione denominata `addTodoList`, che crea un nuovo todoList e lo aggiunge alla matrice. Per vedere come viene chiamata questa funzione, aprire il file di modello denominato todoListTemplate.html, nella cartella modelli. Il seguente codice del modello viene associato un pulsante per la `addTodoList` funzione:

[!code-html[Main](emberjs-template/samples/sample8.html)]

Il controller contiene anche un `error` proprietà, che contiene un messaggio di errore. Ecco il codice del modello per visualizzare il messaggio di errore (anche in todoListTemplate.html):

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a>Route

Router.js definisce le route e il modello predefinito da visualizzare, set di backup dello stato dell'applicazione e corrispondere agli URL di route:

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

TodoListRoute.js carica i dati per il TodoListRoute eseguendo l'override la funzione setupController:

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

Ember Usa le convenzioni di denominazione per associare gli URL, i nomi di route, controller e modelli. Per altre informazioni, vedere [ http://emberjs.com/guides/routing/defining-your-routes/ ](http://emberjs.com/guides/routing/defining-your-routes/) documentazione EmberJS.

## <a name="templates"></a>Modelli

La cartella dei modelli contiene quattro modelli:

- Application.hbs: il modello predefinito che viene eseguito il rendering quando l'applicazione viene avviata.
- About.hbs: il modello per la route "/ circa".
- index.hbs: il modello per la radice "/" route.
- todoList.hbs: il modello per la "/ todo" route.
- \_NavBar.hbs: il modello definisce il menu di navigazione.

Il modello di applicazione funziona come una pagina master. Contiene un'intestazione, un piè di pagina e un "{{outlet}}" per inserire gli altri modelli in base alla route. Per altre informazioni sui modelli di applicazione in Ember, vedere [ http://guides.emberjs.com/v1.10.0/templates/the-application-template// ](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).

Il "/ todoList" modello contiene due espressioni del ciclo for. Il ciclo esterno viene `{{#each controller}}`e il ciclo è `{{#each todos}}`. Il codice seguente illustra un oggetto incorporato `Ember.Checkbox` visualizzare, un oggetto personalizzato `App.TodoItemEditView`e un collegamento con un `deleteTodo` azione.

[!code-html[Main](emberjs-template/samples/sample12.html)]

Il `HtmlHelperExtensions` classe, definita in Controllers/HtmlHelperExensions.cs, definisce un helper funzione memorizzare nella cache e Inserisci modello di file durante **debug** è impostata su **true** nel file Web. config. Questa funzione viene chiamata dal file di visualizzazione ASP.NET MVC definito in Views/Home/App.cshtml:

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

Chiamato senza argomenti, la funzione esegue il rendering di tutti i file di modello nella cartella modelli. È anche possibile specificare una sottocartella o un file di modello specifico.

Quando **debug** viene **false** in Web. config, l'applicazione include l'elemento del bundle "~/bundles/templates". Viene aggiunto questo elemento bundle in BundleConfig.cs, usando la libreria del compilatore Handlebars:

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
