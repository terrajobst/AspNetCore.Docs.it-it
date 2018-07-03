---
uid: single-page-application/overview/templates/backbonejs-template
title: Modello backbone | Microsoft Docs
author: madskristensen
description: Modello di applicazione a singola pagina backbone
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/04/2013
ms.topic: article
ms.assetid: 00aca413-f067-4108-9bd1-cf21e64a2646
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/backbonejs-template
msc.type: authoredcontent
ms.openlocfilehash: 641e149155fbee2655024bec3b76dce5243e7d59
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362505"
---
<a name="backbone-template"></a>Modello backbone
====================
da [Mads Kristensen](https://github.com/madskristensen)

> Il modello di applicazione a singola pagina Backbone è stato scritto da Kazi Manzur Rashid
> 
> [Scaricare il modello di applicazione a singola pagina backbone](https://go.microsoft.com/fwlink/?LinkId=293631)


Il modello backbone SPA è progettato per iniziare a creare rapidamente App web lato client interattivo con [backbone.](http://backbonejs.org/)

Il modello fornisce uno scheletro iniziale per lo sviluppo di un'applicazione di backbone in ASP.NET MVC. Impostazione predefinita fornisce funzionalità di accesso utente di base, tra cui la reimpostazione della password di iscrizione, accesso, utente e la conferma dell'utente con i modelli di base tramite posta elettronica.

Requisiti:

- [Aggiornamento ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)

## <a name="create-a-backbone-template-project"></a>Creare un progetto di modello Backbone

Scaricare e installare il modello facendo clic sul pulsante Download precedente. Il modello viene fornito come un file di Visual Studio Extension (VSIX). Si potrebbe essere necessario riavviare Visual Studio.

Nel **modelli** riquadro, selezionare **modelli installati** ed espandere le **Visual c#** nodo. Sotto **Visual c#**, selezionare **Web**. Nell'elenco dei modelli di progetto, selezionare **applicazione Web ASP.NET MVC 4**. Denominare il progetto e fare clic su **OK**.

![](backbonejs-template/_static/image1.png)

Nel **nuovo progetto** procedura guidata, selezionare progetto SPA backbone.

![](backbonejs-template/_static/image2.png)

Premere CTRL+F5 per compilare ed eseguire l'applicazione senza eseguire il debug oppure premere F5 per eseguire il debug.

![](backbonejs-template/_static/image3.png)

Facendo clic su "Account personale" viene visualizzata la pagina di accesso:

![](backbonejs-template/_static/image4.png)

## <a name="walkthrough-client-code"></a>Procedura dettagliata: Il codice Client

Si inizia con il lato client. Gli script dell'applicazione client si trovano nella cartella ~/Scripts/application. L'applicazione viene scritta [TypeScript](http://www.typescriptlang.org/) (file con estensione TS) che vengono compilati in JavaScript (file con estensione js).

**Applicazione**

`Application` è definito in application.ts. Questo oggetto consente di inizializzare l'applicazione e agisce come spazio dei nomi radice. Gestisce le informazioni di configurazione e lo stato sono condiviso tra l'applicazione, ad esempio se l'utente è connesso.

Il `application.start` metodo consente di creare le visualizzazioni modali e associa i gestori eventi per gli eventi a livello di applicazione, ad esempio accesso dell'utente. Successivamente, crea il router predefiniti e verifica se è specificato alcun URL del lato client. Se non viene reindirizzato all'url predefinito (&! /).

**Eventi**

Gli eventi sono sempre importanti durante lo sviluppo a regime di controllo libero di componenti poco accoppiati. Le applicazioni spesso eseguono più operazioni in risposta a un'azione dell'utente. Backbone fornisce gli eventi predefiniti con i componenti, ad esempio modello, raccolta e visualizzazione. Anziché creare interdipendenze tra questi componenti, il modello Usa un modello "pubblicazione/sottoscrizione": il `events` oggetto, definito in events.ts, funge da un hub eventi per pubblicare e sottoscrivere gli eventi dell'applicazione. Il `events` oggetto è un singleton. Il codice seguente viene illustrato come sottoscrivere un evento e quindi attivare l'evento:

[!code-csharp[Main](backbonejs-template/samples/sample1.cs)]

**Router**

In backbone, un router fornisce metodi per il routing lato client pagine e connettendoli a eventi e azioni. Il modello definisce un solo router, in router.ts. Il router crea le visualizzazioni activable e mantiene lo stato quando si passa le visualizzazioni. (Nella sezione successiva sono descritte le viste activable). Inizialmente, il progetto è disponibili due visualizzazioni fittizie, Home e sulle. Include anche una visualizzazione non trovato, viene visualizzata se la route non è noto.

**Visualizzazioni**

Le viste sono definite nelle ~/Scripts/application o nelle viste. Esistono due tipi di visualizzazioni, visualizzazioni activable e visualizzazioni di finestra di dialogo modale. Viste activable vengono richiamate dal router. Quando viene visualizzata una vista activable, tutte le altre visualizzazioni activable diventano inattivi. Per creare una vista activable, estendere la visualizzazione con il `Activable` oggetto:

[!code-javascript[Main](backbonejs-template/samples/sample2.js)]

Estendere con `Activable` aggiunge due nuovi metodi per la visualizzazione `activate` e `deactivate`. Il router chiama questi metodi per attivare e disattivare le la visualizzazione.

Le visualizzazioni modali vengono implementate come [Twitter Bootstrap](http://twitter.github.com/bootstrap/) le finestre di dialogo modale. Il `Membership` e `Profile` visualizzazioni sono modale. Viste del modello possono essere richiamate da tutti gli eventi dell'applicazione. Ad esempio, nel `Navigation` visualizzazione, fare clic sul collegamento "Account personale" Mostra uno il `Membership` visualizzazione o la `Profile` visualizzazione, a seconda del fatto che l'utente è connesso. Il `Navigation` consente di collegare fare clic sui gestori eventi agli elementi figlio che hanno il `data-command` attributo. Ecco il markup HTML:

[!code-html[Main](backbonejs-template/samples/sample3.html)]

Ecco il codice in navigation.ts per associare gli eventi:

[!code-csharp[Main](backbonejs-template/samples/sample4.cs)]

**Modelli**

I modelli sono definiti in ~/Scripts/application o i modelli. Tutti i modelli presentano tre elementi di base: attributi predefiniti, le regole di convalida e un punto di fine sul lato server. Ecco un esempio tipico:

[!code-javascript[Main](backbonejs-template/samples/sample5.js)]

**Plug-in**

La cartella ~/Scripts/application/lib contiene alcuni utili jQuery plug-in. Il file form.ts definisce un plug-in per l'utilizzo di dati del form. Spesso è necessario serializzare o deserializzare i dati del modulo e visualizzare eventuali errori di convalida del modello. Il plug-in form.ts dispone di metodi, ad esempio `serializeFields`, `deserializeFields`, e `showFieldErrors`. Nell'esempio seguente viene illustrato come serializzare un form a un modello.

[!code-javascript[Main](backbonejs-template/samples/sample6.js)]

Il plug-in flashbar.ts fornisce vari tipi di messaggi con commenti all'utente. I metodi sono `$.showSuccessbar`, `$.showErrorbar` e `$.showInfobar`. Dietro le quinte, Usa gli avvisi di Twitter Bootstrap per mostrare i messaggi vengono animati.

Il plug-in confirm.ts sostituisce il browser confermare finestra di dialogo, anche se l'API è leggermente diverso:

[!code-javascript[Main](backbonejs-template/samples/sample7.js)]

## <a name="walkthrough-server-code"></a>Procedura dettagliata: Codice del Server

Ora esaminiamo il lato server.

**Controller**

In un'applicazione a pagina singola, il server ha solo un ruolo piccolo nell'interfaccia utente. In genere, il server esegue il rendering della pagina iniziale e quindi invia e riceve i dati JSON.

Il modello dispone di due controller MVC: `HomeController` esegue il rendering della pagina iniziale, e `SupportsController` viene usato per verificare che i nuovi account utente e reimpostare le password. Tutti gli altri controller nel modello sono i controller API Web ASP.NET, che inviano e ricevono i dati JSON. Per impostazione predefinita, usare il nuovo controller di `WebSecurity` classe per eseguire attività relative all'utente. Tuttavia, dispongono anche costruttori facoltativi che consentono di passare in delegati per eseguire queste attività. Ciò rende le operazioni di testing e consente di sostituire `WebSecurity` con altro, usando un contenitore IoC. Ecco un esempio:

[!code-csharp[Main](backbonejs-template/samples/sample8.cs)]

## <a name="views"></a>Visualizzazioni

Le viste sono progettate per essere modulare: ogni sezione di una pagina ha la propria visualizzazione dedicata. In un'applicazione a pagina singola, è comune per includere viste che non hanno alcun controller corrispondente. È possibile includere una visualizzazione chiamando `@Html.Partial('myView')`, ma questo modo si ottengono noioso. Per semplificare questa operazione, il modello definisce un metodo helper, `IncludeClientViews`, che esegue il rendering di tutte le viste in una cartella specificata:

[!code-cshtml[Main](backbonejs-template/samples/sample9.cshtml)]

Se non viene specificato il nome della cartella, il nome della cartella predefinita è "ClientViews". Se la visualizzazione client usa anche le visualizzazioni parziali, assegnare un nome della visualizzazione parziale con un carattere di sottolineatura (ad esempio, `_SignUp`). Il `IncludeClientViews` metodo esclude le visualizzazioni il cui nome inizia con un carattere di sottolineatura. Per includere una visualizzazione parziale nella vista del client, chiamare `Html.ClientView('SignUp')` invece di `Html.Partial('_SignUp')`.

**L'invio di posta elettronica**

Per inviare posta elettronica, il modello Usa [Postal](http://aboutcode.net/postal). Tuttavia, è un'astrazione Postal dal resto del codice con il `IMailer` interfaccia, in modo che è facilmente possibile sostituirlo con un'altra implementazione. Modelli di posta elettronica si trovano nella cartella visualizzazioni/messaggi di posta elettronica. Indirizzo di posta elettronica del mittente è specificato nel file Web. config, nelle `sender.email` chiave del **appSettings** sezione. Inoltre, quando `debug="true"` in Web. config, l'applicazione non richiede conferma tramite posta elettronica dell'utente, per velocizzare lo sviluppo.

## <a name="github"></a>GitHub

È anche possibile trovare il modello di applicazione a singola pagina backbone sul [GitHub](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa).
