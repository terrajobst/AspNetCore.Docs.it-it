---
uid: single-page-application/overview/templates/hottowel-template
title: Modello degli asciugamani a caldo | Documenti Microsoft
author: madskristensen
description: Modello HotTowel
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/09/2013
ms.topic: article
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: dbd037c2469d326a3d3248ca07492ed9eb93e225
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="hot-towel-template"></a>Modello degli asciugamani a caldo
====================
da [Mads Kristensen](https://github.com/madskristensen)

> Il modello di MVC degli asciugamani a caldo è scritto da John Papa
> 
> Scegliere la versione da scaricare:
> 
> [Modello MVC per accesso frequente asciugamani per Visual Studio 2012](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [Modello MVC per accesso frequente asciugamani per Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> Scelta degli asciugamani: Perché non si desidera utilizzare l'autenticazione SPA senza uno!


Per compilare un SPA ma non è possibile decidere dove iniziare? Utilizzare degli asciugamani attivo e in secondi è necessario un SPA e tutti gli strumenti che necessari per creare su di esso!

Scelta degli asciugamani Crea punto di partenza ideale per la creazione di un'applicazione a pagina singola (SPA) con ASP.NET. All'esterno della casella si fornisce una struttura modulare per codice, la visualizzazione, associazione dati, gestione di dati complessi e lo stile semplice ma elegante. Scelta degli asciugamani offre tutto ciò che occorre per compilare un SPA, è possibile concentrarsi sull'applicazione, non l'impianto.

> Ulteriori informazioni sulla compilazione di un SPA da [John Papa video, esercitazioni e corsi Pluralsight](http://johnpapa.net/spa?vsix).


## <a name="application-structure"></a>Struttura dell'applicazione

Scelta degli asciugamani SPA fornisce una cartella di App che contiene i file JavaScript e HTML che definiscono l'applicazione.

All'interno della cartella di App:

- durandal
- servizi
- ViewModel
- visualizzazioni

La cartella di App contiene una raccolta di moduli. Questi moduli incapsulano la funzionalità e dichiarano dipendenze in altri moduli. La cartella views contiene il codice HTML per l'applicazione e la cartella ViewModel contiene la logica di presentazione per le viste (un comune modello MVVM). La cartella servizi è ideale per l'inserimento di tutti i servizi comuni che l'applicazione potrebbe essere necessario, ad esempio il recupero dei dati HTTP o l'interazione di archiviazione locale. È comune per più ViewModel di riutilizzare codice dei moduli del servizio.

## <a name="aspnet-mvc-server-side-application-structure"></a>Struttura delle applicazioni lato Server ASP.NET MVC

Scelta degli asciugamani compila in base alla struttura di ASP.NET MVC potente e familiare.

- App\_avviare
- Content
- Controllers
- Modelli
- Script
- Visualizzazioni

## <a name="featured-libraries"></a>Librerie in primo piano

- ASP.NET MVC
- API Web ASP.NET
- Ottimizzazione di ASP.NET Web - creazione di bundle e riduzione
- [Breeze.js](http://Breezejs.com) -gestione di dati complessi
- [Durandal.js](http://Durandaljs.com) -navigazione e composizione della vista
- [Knockout.js](http://Knockoutjs.com) -i data binding
- [Require](http://requirejs.org) -modularità con AMD e ottimizzazione
- [Toastr.js](http://jpapa.me/c7toastr) -messaggi popup
- [Twitter Bootstrap](http://twitter.github.com/bootstrap/) - applicazione di stili CSS affidabile

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a>L'installazione tramite il modello SPA degli asciugamani a caldo di Visual Studio 2012

Scelta degli asciugamani possono essere installato come un modello di Visual Studio 2012. Fare clic su `File`  |  `New Project` e scegliere `ASP.NET MVC 4 Web Application`. Selezionare quindi il ' applicazione a pagina singola degli asciugamani a caldo "modello ed eseguire!

## <a name="installing-via-the-nuget-package"></a>L'installazione tramite il pacchetto NuGet

Scelta degli asciugamani sono anche un pacchetto NuGet che aumenta di un progetto ASP.NET MVC vuoto esistente. È sufficiente installare tramite Nuget e quindi eseguire!

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a>Come creare su asciugamani frequente?

Iniziare semplicemente l'aggiunta di codice.

1. Aggiungere il codice lato server, preferibilmente Entity Framework e WebAPI (che caratterizzano effettivamente con Breeze.js)
2. Aggiungere visualizzazioni per il `App/views` cartella
3. Aggiungere ViewModel per il `App/viewmodels` cartella
4. Aggiungere codice HTML e Knockout associazioni di dati alle nuove visualizzazioni
5. Aggiornare le route di navigazione in `shell.js`

## <a name="walkthrough-of-the-htmljavascript"></a>Procedura dettagliata del codice HTML/JavaScript

### <a name="viewshottowelindexcshtml"></a>Views/HotTowel/index.cshtml

cshtml è la route iniziale e visualizzazione per l'applicazione MVC. Contiene tutti i tag meta standard, i collegamenti di css e JavaScript riferimenti che previsto. Il corpo contiene un singolo `<div>` che è in tutto il contenuto (visualizzazioni) di cui verrà inserito quando vengono richiesti. Il `@Scripts.Render` Usa Require.js per eseguire il punto di ingresso per il codice dell'applicazione, contenuta nella `main.js` file. La schermata iniziale viene fornita per illustrare come creare una schermata durante il caricamento dell'app.

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a>App/main.js

Il `main.js` file contiene il codice che verrà eseguito non appena viene caricata l'app. Si tratta in cui si desidera definire i percorsi di navigazione, impostare l'avvio delle viste e di eseguire qualsiasi programma di installazione o l'avvio, ad esempio priming i dati dell'applicazione.

Il `main.js` file definisce molti dei moduli del durandal per avviare kick dell'applicazione. L'istruzione di definizione consente di risolvere le dipendenze di moduli in modo che siano disponibili per la funzione. Innanzitutto, sono abilitati i messaggi di debug quale inviare messaggi sugli eventi che sta eseguendo l'applicazione alla finestra della console. Il codice dell'App indica durandal framework per avviare l'applicazione. Le convenzioni vengono impostate in modo che durandal conosce tutte le visualizzazioni e ViewModel contenuti nelle stesse cartelle denominate, rispettivamente. Infine, il `app.setRoot` avvia carica il `shell` utilizzando un `entrance` animazione.

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a>Visualizzazioni

Viste, vedere il `App/views` cartella.

### <a name="shellhtml"></a>Shell.HTML

Il `shell.html` contiene il layout dello schema per il codice HTML. Tutte le altre viste verranno create in un punto sul lato del `shell` visualizzazione. Scelta degli asciugamani fornisce un `shell` con tre di queste aree: un'intestazione, un'area di contenuto e un piè di pagina. Ognuna di queste aree è caricato con contenuto modulo altre viste quando richiesto.

Il `compose` binding per l'intestazione e piè di pagina sono codificati in asciugamani frequente in modo che punti al `nav` e `footer` viste, rispettivamente. L'associazione di composizione per la sezione `#content` è associato il `router` elemento attivo del modulo. In altre parole, quando fa clic su un collegamento di navigazione è vista corrispondente viene caricato in questa area.

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a>nav.html

Il `nav.html` contiene i collegamenti di navigazione per l'autenticazione SPA. Si tratta di dove sia possibile posizionare la struttura di menu, ad esempio. Spesso si tratta di dati associati (tramite Knockout) per il `router` modulo per visualizzare la navigazione in cui è definito il `shell.js`. Knockout ricerca per l'associazione di dati degli attributi e associa questi il `shell` viewmodel per visualizzare gli itinerari di navigazione e per mostrare un progressbar (tramite Twitter Bootstrap) se il `router` modulo è impegnato passare da una vista a altra (vedere `router.isNavigating`).

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a>Home.HTML e details.html

Queste viste contengono HTML per visualizzazioni personalizzate. Quando il `home` collegare il `nav` si fa clic sul menu della visualizzazione, il `home` vista verrà posizionata nell'area del contenuto del `shell` visualizzazione. Queste viste possono essere aumentate o sostituite con visualizzazioni personalizzate.

### <a name="footerhtml"></a>footer.html

Il `footer.html` contiene codice HTML visualizzato nel piè di pagina, nella parte inferiore del `shell` visualizzazione.

## <a name="viewmodels"></a>ViewModel

ViewModel presenti nel `App/viewmodels` cartella.

### <a name="shelljs"></a>shell.js

Il `shell` viewmodel contiene proprietà e funzioni che sono associate ai `shell` visualizzazione. Spesso si tratta in cui si trovano le associazioni di navigazione di menu (vedere il `router.mapNav` logica).

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a>Home.js e details.js

Questi ViewModel contengono le proprietà e funzioni che sono associate ai `home` visualizzazione. inoltre contiene la logica di presentazione per la visualizzazione ed è l'associazione tra i dati e la visualizzazione.

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a>Servizi

I servizi si trovano nella cartella di App e servizi. In teoria è stato possibile collocare i servizi, ad esempio un modulo dataservice, che è responsabile per ottenere e inviare i dati remoti, future.

### <a name="loggerjs"></a>logger.js

Scelta degli asciugamani fornisce un `logger` modulo nella cartella servizi. Il `logger` modulo è ideale per la registrazione messaggi per la console e per l'utente nel pop-up avvisi popup.
