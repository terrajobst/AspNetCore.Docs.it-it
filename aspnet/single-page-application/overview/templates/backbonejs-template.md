---
uid: single-page-application/overview/templates/backbonejs-template
title: Modello backbone | Documenti Microsoft
author: madskristensen
description: Modello SPA backbone.js
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/04/2013
ms.topic: article
ms.assetid: 00aca413-f067-4108-9bd1-cf21e64a2646
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/backbonejs-template
msc.type: authoredcontent
ms.openlocfilehash: 3b8eabd3cefcb96dc40bbf6cc6e3ee81accb0d7c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="backbone-template"></a>Modello backbone
====================
da [Mads Kristensen](https://github.com/madskristensen)

> Il modello di SPA Backbone è stato scritto dal Kazi Manzur Rashid
> 
> [Scaricare il modello SPA Backbone.js](https://go.microsoft.com/fwlink/?LinkId=293631)


Il modello di SPA Backbone.js è progettato per iniziare a creare rapidamente applicazioni web sul lato client interattivo utilizzando [Backbone.js.](http://backbonejs.org/)

Il modello fornisce uno scheletro iniziale per lo sviluppo di un'applicazione Backbone.js in ASP.NET MVC. Fornisce funzionalità di accesso utente di base, tra cui la reimpostazione della password di iscrizione, accedi, utente e la conferma dell'utente con i modelli di posta elettronica di base predefinita.

Requisiti:

- [Aggiornamento ASP.NET e Web strumenti 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)

## <a name="create-a-backbone-template-project"></a>Creare un progetto di modello Backbone

Scaricare e installare il modello fare clic sul pulsante Download. Il modello viene fornito come un file di Visual Studio Extension (VSIX). Si potrebbe essere necessario riavviare Visual Studio.

Nel **modelli** riquadro, selezionare **modelli installati** ed espandere il **Visual c#** nodo. In **Visual c#**selezionare **Web**. Nell'elenco dei modelli di progetto, selezionare **applicazione Web ASP.NET MVC 4**. Denominare il progetto e fare clic su **OK**.

![](backbonejs-template/_static/image1.png)

Nel **nuovo progetto** procedura guidata, selezionare progetto SPA Backbone.js.

![](backbonejs-template/_static/image2.png)

Premere Ctrl + F5 per compilare ed eseguire l'applicazione senza debug oppure premere F5 per eseguire il debug.

![](backbonejs-template/_static/image3.png)

Facendo clic su "My Account" viene visualizzata la pagina di accesso:

![](backbonejs-template/_static/image4.png)

## <a name="walkthrough-client-code"></a>Procedura dettagliata: Il codice Client

Si inizia con il lato client. Gli script dell'applicazione client si trovano nella cartella ~/Scripts/application. L'applicazione viene scritta [TypeScript](http://www.typescriptlang.org/) (file con estensione TS) che vengono compilati in JavaScript (file con estensione js).

**Applicazione**

`Application`è definito in application.ts. Questo oggetto consente di inizializzare l'applicazione e opera come spazio dei nomi radice. Gestisce le informazioni di configurazione e dello stato condiviso tra l'applicazione, ad esempio se l'utente è connesso.

Il `application.start` metodo crea le visualizzazioni modali e i gestori eventi per gli eventi a livello di applicazione, ad esempio account utente. Successivamente, il router predefinito crea e controlla se è stato specificato alcun URL sul lato client. Se non viene reindirizzato all'url predefinito (#! /).

**Eventi**

Gli eventi sono sempre importanti durante lo sviluppo loosely coupled componenti. Spesso, le applicazioni eseguono più operazioni in risposta a un'azione dell'utente. Backbone fornisce gli eventi predefiniti con i componenti, ad esempio modello, raccolta e visualizzazione. Anziché creare interdipendenze tra questi componenti, il modello utilizza un modello "pubblicazione/sottoscrizione": il `events` oggetto, definito in events.ts, funge da un hub di eventi per la pubblicazione e sottoscrizione agli eventi dell'applicazione. Il `events` oggetto è un singleton. Il codice seguente viene illustrato come sottoscrivere un evento e quindi attivare l'evento:

[!code-csharp[Main](backbonejs-template/samples/sample1.cs)]

**Router**

In Backbone.js, un router fornisce metodi per il routing delle pagine sul lato client e la loro connessione alle azioni ed eventi. Il modello definisce un singolo router, in router.ts. Il router crea le visualizzazioni activable e mantiene lo stato quando si passa le visualizzazioni. (Activable viste sono descritti nella sezione successiva). Inizialmente, il progetto è disponibili due visualizzazioni fittizie, Home e sulle. Include inoltre una visualizzazione non trovato, viene visualizzata se la route non è noto.

**Visualizzazioni**

Le viste vengono definite in ~/Scripts/application e viste. Esistono due tipi di visualizzazioni, activable e le visualizzazioni di finestra di dialogo modale. Viste activable vengono richiamate dal router. Quando viene visualizzata una vista activable, tutte le altre visualizzazioni activable diventano inattivi. Per creare una vista activable, estendere la vista con il `Activable` oggetto:

[!code-javascript[Main](backbonejs-template/samples/sample2.js)]

Estensione con `Activable` aggiunge due nuovi metodi per la visualizzazione, `activate` e `deactivate`. Il router chiama questi metodi per attivare e disattivare la visualizzazione.

Le visualizzazioni modali vengono implementate come [Twitter Bootstrap](http://twitter.github.com/bootstrap/) le finestre di dialogo modale. Il `Membership` e `Profile` le visualizzazioni sono le visualizzazioni modali. Viste del modello possono essere chiamate da tutti gli eventi dell'applicazione. Ad esempio, nel `Navigation` , facendo clic sul collegamento "Account personale" Visualizza uno il `Membership` visualizzazione o `Profile` visualizzazione, a seconda se l'utente è connesso. Il `Navigation` collega fare clic sui gestori eventi per tutti gli elementi figlio che hanno il `data-command` attributo. Di seguito è riportato il markup HTML:

[!code-html[Main](backbonejs-template/samples/sample3.html)]

Ecco il codice in navigation.ts per associare gli eventi:

[!code-csharp[Main](backbonejs-template/samples/sample4.cs)]

**Modelli**

I modelli vengono definiti in ~/Scripts/application o i modelli. Tutti i modelli dispongano di tre operazioni di base: attributi predefiniti, le regole di convalida e un punto di fine sul lato server. Di seguito è riportato un esempio tipico:

[!code-javascript[Main](backbonejs-template/samples/sample5.js)]

**Plug-in**

La cartella ~/Scripts/application/lib contiene alcuni utili jQuery plug-in. Il file form.ts definisce un plug-in per l'utilizzo di dati del form. Spesso è necessario serializzare o deserializzare i dati del form e visualizzare gli errori di convalida del modello. Il plug-in form.ts dispone di metodi, ad esempio `serializeFields`, `deserializeFields`, e `showFieldErrors`. Nell'esempio seguente viene illustrato come serializzare un form a un modello.

[!code-javascript[Main](backbonejs-template/samples/sample6.js)]

Il plug-in flashbar.ts fornisce vari tipi di messaggi di feedback all'utente. I metodi sono `$.showSuccessbar`, `$.showErrorbar` e `$.showInfobar`. Dietro le quinte, Usa gli avvisi di Bootstrap di Twitter per mostrare i messaggi correttamente animati.

Il plug-in confirm.ts sostituisce il browser confermare finestra di dialogo, anche se l'API è leggermente diverso:

[!code-javascript[Main](backbonejs-template/samples/sample7.js)]

## <a name="walkthrough-server-code"></a>Procedura dettagliata: Codice di Server

Ora esaminiamo il lato server.

**Controller**

In un'applicazione a singola pagina, il server ha solo un ruolo piccolo nell'interfaccia utente. In genere, il server esegue il rendering della pagina iniziale e quindi invia e riceve i dati JSON.

Il modello dispone di due controller MVC: `HomeController` rendering della pagina iniziale, e `SupportsController` viene utilizzato per verificare i nuovi account utente e reimpostare le password. Tutti gli altri controller nel modello sono controller API Web ASP.NET, che inviano e ricevono i dati JSON. Per impostazione predefinita, usano il nuovo controller di `WebSecurity` classe per eseguire operazioni relative agli utenti. Tuttavia, dispongono anche di costruttori facoltativi che consentono di passare i delegati per queste attività. Ciò rende le operazioni di testing e consente di sostituire `WebSecurity` con un altro titolo, utilizzando un contenitore di inversione di controllo. Ecco un esempio:

[!code-csharp[Main](backbonejs-template/samples/sample8.cs)]

## <a name="views"></a>Visualizzazioni

Le viste sono progettate per essere modulare: ogni sezione di una pagina ha una propria vista dedicata. In un'applicazione a pagina singola, è comune per includere viste che non contengono alcun controller corrispondente. È possibile includere una vista chiamando `@Html.Partial('myView')`, ma ottiene noiosa. Per semplificare questa operazione, il modello definisce un metodo di supporto, `IncludeClientViews`, che esegue il rendering di tutte le viste in una cartella specificata:

[!code-cshtml[Main](backbonejs-template/samples/sample9.cshtml)]

Se il nome della cartella non è specificato, il nome della cartella predefinita è "ClientViews". Se la visualizzazione del client utilizza inoltre le visualizzazioni parziali, assegnare il nome della visualizzazione parziale con un carattere di sottolineatura (ad esempio, `_SignUp`). Il `IncludeClientViews` metodo esclude qualsiasi visualizzazione il cui nome inizia con un carattere di sottolineatura. Per includere una visualizzazione parziale nella vista del client, chiamare `Html.ClientView('SignUp')` anziché `Html.Partial('_SignUp')`.

**L'invio di posta elettronica**

Per inviare posta elettronica, il modello utilizza [postale](http://aboutcode.net/postal). Tuttavia, Postal estratti dal resto del codice con il `IMailer` interfaccia, in modo che è possibile sostituire facilmente con un'altra implementazione. I modelli di messaggio di posta elettronica si trovano nella cartella viste/messaggi di posta elettronica. Indirizzo di posta elettronica del mittente è incluso nel file Web. config, il `sender.email` la chiave del **appSettings** sezione. Inoltre, quando `debug="true"` in Web. config, l'applicazione non richiede conferma tramite posta elettronica all'utente, per velocizzare lo sviluppo.

## <a name="github"></a>GitHub

È inoltre possibile trovare il modello di SPA Backbone.js nel [GitHub](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa).
