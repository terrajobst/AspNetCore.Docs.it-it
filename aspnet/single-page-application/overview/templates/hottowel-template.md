---
uid: single-page-application/overview/templates/hottowel-template
title: Modello di hot Towel | Microsoft Docs
author: madskristensen
description: Modello HotTowel
ms.author: riande
ms.date: 02/09/2013
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: b80b5940b5733a0ae2af3491ccdfa05b84b50343
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823838"
---
<a name="hot-towel-template"></a>Modello hot Towel
====================
da [Mads Kristensen](https://github.com/madskristensen)

> Il modello di MVC Hot Towel è scritto da John Papa
> 
> Scegliere la versione da scaricare:
> 
> [Hot Towel modello MVC per Visual Studio 2012](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [Hot Towel modello MVC per Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> Hot Towel: Perché non si vuole passare per l'applicazione a singola pagina senza uno!


Per compilare un'applicazione a singola pagina, ma non è possibile decidere dove iniziare? Usare Hot Towel e in pochi secondi si otterrà un'applicazione a singola pagina e tutti gli strumenti che necessari per creare su di esso.

Hot Towel crea un ottimo punto di partenza per la creazione di un applicazione a pagina singola (SPA) con ASP.NET. Impostazione predefinita si fornisce una struttura modulare per codice, navigazione nella visualizzazione, l'associazione dati, gestione avanzata dei dati e lo stile semplice ma elegante. Hot Towel fornisce tutto ciò che occorre per compilare un'applicazione a singola pagina, è possibile concentrarsi sull'app, non le operazioni di base.

> Altre informazioni sulla creazione di un'applicazione a singola pagina dalla [John Papa video, esercitazioni e i corsi Pluralsight](http://johnpapa.net/spa?vsix).


## <a name="application-structure"></a>Struttura dell'applicazione

Hot Towel SPA fornisce una cartella dell'App che contiene i file JavaScript e HTML che definiscono l'applicazione.

All'interno della cartella di App:

- Durandal
- servizi
- ViewModel
- visualizzazioni

La cartella dell'App contiene una raccolta di moduli. Questi moduli includono funzionalità e dichiarano le dipendenze su altri moduli. La cartella views conterrà il codice HTML per l'applicazione e la cartella ViewModel contiene la logica di presentazione per le viste (un pattern MVVM comune). La cartella services è ideale per l'inserimento di tutti i servizi comuni, ad esempio il recupero dei dati HTTP o l'interazione dell'archiviazione locale potrebbe essere necessario all'applicazione. È comune per più ViewModel per riutilizzare il codice dei moduli del servizio.

## <a name="aspnet-mvc-server-side-application-structure"></a>Struttura delle applicazioni lato Server ASP.NET MVC

Hot Towel si basa sulla struttura di ASP.NET MVC potente e familiare.

- App\_Start
- Content
- Controllers
- Modelli
- Script
- Visualizzazioni

## <a name="featured-libraries"></a>Librerie in primo piano

- ASP.NET MVC
- API Web ASP.NET
- Ottimizzazione Web ASP.NET - creazione di bundle e minimizzazione
- [Breeze.js](http://Breezejs.com) -gestione avanzata dei dati
- [Durandal](http://Durandaljs.com) -navigazione e composizione della vista
- [Knockout. js](http://Knockoutjs.com) -associazioni dati
- [Require](http://requirejs.org) -modularità con processori AMD e ottimizzazione
- [Toastr.js](http://jpapa.me/c7toastr) -messaggi popup
- [Twitter Bootstrap](http://twitter.github.com/bootstrap/) : applicazione di stili CSS affidabile

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a>Installazione tramite il modello di applicazione a singola pagina di Visual Studio 2012 Hot Towel

Hot Towel può essere installato come un modello di Visual Studio 2012. Fare semplicemente clic `File`  |  `New Project` e scegliere `ASP.NET MVC 4 Web Application`. Quindi selezionare il ' applicazione a pagina singola Towel Hot "modello ed eseguire.

## <a name="installing-via-the-nuget-package"></a>Installazione tramite il pacchetto NuGet

Hot Towel è anche un pacchetto NuGet che aggiunge un progetto MVC ASP.NET vuoto esistente. È sufficiente installare tramite Nuget e quindi eseguire.

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a>Come creare in Hot Towel?

È sufficiente iniziare ad aggiungere codice!

1. Aggiungere il codice del lato server, preferibilmente Entity Framework e API Web (che caratterizzano realmente con Breeze.js)
2. Aggiungere visualizzazioni al `App/views` cartella
3. Aggiungere ViewModel di `App/viewmodels` cartella
4. Aggiungere HTML e Knockout associazioni dati per le nuove viste
5. Aggiornare le route di navigazione in `shell.js`

## <a name="walkthrough-of-the-htmljavascript"></a>Procedura dettagliata di HTML/JavaScript

### <a name="viewshottowelindexcshtml"></a>Views/HotTowel/index.cshtml

index. cshtml è la route e visualizzazione per l'applicazione MVC inizio. Contiene tutte le tag meta standard, css i collegamenti e riferimenti di JavaScript che ci si aspetta. Il corpo contiene un singolo `<div>` che è in tutto il contenuto (visualizzazioni) di cui verrà inserito quando vengono richiesti. Il `@Scripts.Render` Usa Require. js per eseguire il punto di ingresso per il codice dell'applicazione, contenuta nel `main.js` file. La schermata iniziale viene fornita per illustrare come creare una schermata mentre caricate dall'app.

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a>App/Main.js

Il `main.js` file contiene il codice che verrà eseguito non appena viene caricato l'app. Si tratta in cui si desidera definire le route di navigazione, impostare l'avvio di visualizzazioni ed eseguire qualsiasi programma di installazione/di avvio automatico, ad esempio l'inizializzazione dei dati dell'applicazione.

Il `main.js` file definiti diversi attributi dei moduli del durandal per avviare l'inizio dell'applicazione. L'istruzione di definizione consente di risolvere le dipendenze di moduli in modo che siano disponibili per la funzione. Prima di tutto i messaggi di debug sono abilitati, quali invio di messaggi sui quali eventi eseguite dall'applicazione alla finestra della console. Il codice di App indica durandal framework per avviare l'applicazione. Le convenzioni vengono impostate in modo che durandal conosce tutte le visualizzazioni e ViewModel sono contenuti nelle stesse cartelle denominate, rispettivamente. Infine, il `app.setRoot` dà il via a carichi le `shell` usando un oggetto predefinito `entrance` animazione.

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a>Visualizzazioni

Le visualizzazioni si trovano nella `App/views` cartella.

### <a name="shellhtml"></a>Shell.HTML

Il `shell.html` contiene layout master per il codice HTML. Tutte le altre visualizzazioni verranno combinate in una posizione sul lato del `shell` visualizzazione. Hot Towel fornisce un `shell` con queste tre aree: un'intestazione, un'area di contenuto e un piè di pagina. Ognuna di queste aree è dotata di contenuto formare altre viste quando richiesto.

Il `compose` associazioni per l'intestazione e piè di pagina sono hardcoded in Hot Towel in modo che punti la `nav` e `footer` Visualizza, rispettivamente. L'associazione di compose per la sezione `#content` è associato il `router` elemento attivo del modulo. In altre parole, quando si fa clic su un collegamento di navigazione è visualizzazione corrispondente viene caricato in questa area.

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a>NAV.HTML

Il `nav.html` contiene i collegamenti di navigazione per l'applicazione a singola pagina. Si tratta in cui può essere inserita nella struttura di menu, ad esempio. Spesso si tratta di dati associati (usando Knockout) per il `router` modulo per visualizzare la navigazione è definito nel `shell.js`. Knockout Azure cerca di associare i dati degli attributi e associa quelli per il `shell` viewmodel per visualizzare le route di navigazione e per mostrare un progressbar (tramite Twitter Bootstrap) se il `router` modulo è in corso lo spostamento da una visualizzazione a un altro (vedere `router.isNavigating`).

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a>Home. HTML e details.html

Queste viste contengono HTML per le visualizzazioni personalizzate. Quando il `home` clic sul collegamento nella `nav` si fa clic sul menu della visualizzazione, il `home` verrà inserita nell'area del contenuto della visualizzazione il `shell` visualizzazione. Queste viste possono essere aumentate o sostituite con visualizzazioni personalizzate.

### <a name="footerhtml"></a>Footer.HTML

Il `footer.html` contiene codice HTML visualizzato nel piè di pagina, in fondo il `shell` visualizzazione.

## <a name="viewmodels"></a>ViewModel

ViewModel vengono trovati nel `App/viewmodels` cartella.

### <a name="shelljs"></a>Shell.js

Il `shell` viewmodel contiene le proprietà e funzioni che sono associate ai `shell` visualizzazione. Spesso si tratta di dove si trovano le associazioni di navigazione di menu (vedere il `router.mapNav` per la logica).

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a>details.js e js

Tali ViewModel contengono le proprietà e funzioni che sono associate ai `home` visualizzazione. inoltre contiene la logica di presentazione per la visualizzazione ed è l'associazione tra i dati e la visualizzazione.

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a>Servizi

I servizi sono disponibili nella cartella/servizi App. In teoria i servizi futuri, ad esempio un modulo dataservice, che è responsabile per il recupero e la registrazione dei dati remota, è possibile attivare.

### <a name="loggerjs"></a>logger.js

Hot Towel fornisce un `logger` modulo nella cartella dei servizi. Il `logger` modulo è ideale per i messaggi di registrazione nella console e per l'utente nella finestra popup gli avvisi popup.
