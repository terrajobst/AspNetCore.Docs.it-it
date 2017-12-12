---
uid: single-page-application/overview/templates/breezeangular-template
title: Modello molto semplice/angolare | Documenti Microsoft
author: madskristensen
description: Modello di applicazione a pagina singola semplicissimo/angolare
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/08/2013
ms.topic: article
ms.assetid: db31e909-563a-4516-aadd-62aa210ac7e4
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/breezeangular-template
msc.type: authoredcontent
ms.openlocfilehash: faf28a510a83b7fa07585904344176601c2e1f34
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="breezeangular-template"></a>Modello molto semplice/angolare
====================
da [Mads Kristensen](https://github.com/madskristensen)

> Il modello MVC semplicissimo/angolare è stato scritto dal Ward Bell
> 
> [Scaricare il modello MVC semplicissimo/Angular](https://go.microsoft.com/fwlink/?LinkId=286437)


[AngularJS](http://angularjs.org) è una libreria open source da Google per la compilazione di applicazioni a pagina singola (stabilimenti termali). Offre data binding, l'inserimento di dipendenze e gestione dello schermo. Combinarlo con [BreezeJS](http://www.breezejs.com/?utm_source=ms-spa), un'altra libreria open source per la modellazione di dati e la gestione dei dati, nonché disporre gli elementi essenziali per un'applicazione client HTML/JavaScript eccezionale.

Il modello molto semplice/angolare SPA è una variazione sul [modello Knockout.js SPA](../introduction/knockoutjs-template.md) incluso nell'aggiornamento 2012.2 di strumenti Web di ASP.NET. Se si dispone di Visual Studio, è necessario un esempio SPA attivo e in esecuzione in meno di 60 secondi.

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningTodoPage.png)

Per tutti gli altri apparentemente, l'applicazione è molto simile al modello di SPA Knockout.js. Ma è piuttosto diverso, dietro le quinte. Il modello Knockout.js utilizza Knockout per l'associazione di dati e AJAX non elaborato per l'accesso ai dati. Il modello molto semplice/angolare utilizza angolare per l'associazione di dati e molto semplice per accedere ai dati. Queste librerie abilitare funzionalità aggiuntive, tra cui cronologia e la navigazione.

Di seguito è la pagina dell'applicazione su:

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningAboutPage.png)

Questa pagina vengono visualizzati in esecuzione di un log eventi durante la sessione utente corrente, tra cui:

- Paging. Si noti la creazione di controller Todo #2 e #7.
- Le query remote (n. 3) e le query di cache locale (#7).
- Salvataggio di nuova (n. 5, &#6;) e modificare le entità (4).
- Modifiche è state convalidate nel client (9), pertanto l'utente possa correggere eventuali errori prima di eseguire il commit delle modifiche al database.

È presente più di esplorare in questo modello, tra cui:

- Caricamento dinamico dei modelli di visualizzazione HTML.
- Associazione di dati personalizzati tramite angolare "direttive".
- Attacco intrusivo nel codice di modularità e delle dipendenze.
- I filtri di query, ordinamenti, paging, proiezioni e inclusione di entità correlate.
- La condivisione dei dati in più schermate.
- Salvataggio delle modifiche più come una singola transazione.
- Le regole di convalida propagate automaticamente dal server al client JavaScript.

Ma veniamo al dunque.

## <a name="create-a-breezeangular-template-project"></a>Creare un progetto di modello molto semplice/Angular

Scaricare e installare il modello fare clic sul pulsante Download. Il modello viene fornito come un file di Visual Studio Extension (VSIX). Si potrebbe essere necessario riavviare Visual Studio.

Nel **modelli** riquadro, selezionare **modelli installati** ed espandere il **Visual c#** nodo. In **Visual c#**selezionare **Web**. Nell'elenco dei modelli di progetto, selezionare **applicazione Web ASP.NET MVC 4**. Denominare il progetto e fare clic su **OK**.

Nel **nuovo progetto** procedura guidata, selezionare **SPA angolare semplicissimo**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeNgSpaTemplate.png)

Premere Ctrl + F5 per compilare ed eseguire l'applicazione senza debug oppure premere F5 per eseguire il debug.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrLogin.png)

Quando l'applicazione esegue la prima volta, viene visualizzata una schermata di accesso. Fare clic sul collegamento "Accesso" e una nuova pagina glides nella visualizzazione, in cui è possibile immettere un nome utente e password. (Le pagine di accesso e la registrazione vengono compilate mediante ASP.NET MVC.) Quando si invia il modulo di registrazione, il server genera un elenco di attività con due elementi per l'account. Quindi li presenta all'utente in una nota di colore giallo.

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

A questo punto si è la terra di SPA. Tutto ciò che viene visualizzata e verificarsi durante la modifica di Todos è sottoposto a rendering e gestita sul client con l'aiuto di ritaglio e molto semplice. Esplorare l'applicazione come utente... ma con occhio dello sviluppatore. Usare gli strumenti di sviluppo nel browser per acquisire il traffico di rete. (In Internet Explorer: premere F12, selezionare il **rete** scheda e fare clic su **iniziare ad acquisire**.) Ora provare le operazioni seguenti:

- Aggiungere un nuovo elemento di attività.
- Fare clic sull'etichetta e modificare il titolo dell'elemento Todo
- Selezionare una casella di controllo per contrassegnare l'elemento completato. Si noti che la casella di testo è disabilitato, pertanto il titolo non è più modificabile.
- Fare clic sulla x a destra dell'etichetta. L'elemento viene rimosso e viene eliminata dal database.
- Selezionare un altro elemento e deselezionare il titolo. Si otterrà un errore di convalida che il titolo è obbligatorio. Dopo una breve pausa, viene ripristinato il titolo precedente.
- Digitare un titolo sia estremamente lungo. Si otterrà un errore di convalida diverso che il titolo è troppo lungo.
- Fare clic sul pulsante "Aggiungi Todo List". Un nuovo elenco visualizzato a sinistra dell'elenco precedente.
- Provare a usare le convalide di lunghezza e il titolo del TodoList, attivazione relativo richiesto.
- Fare clic nella casella di testo del titolo per cancellare il messaggio di errore.
- Fare clic su "x" nell'angolo superiore destro per eliminare l'elenco di attività e il relativo todos del cerchio.
- Fare clic sul collegamento "About" in alto a destra per visualizzare un log di queste attività.

La logica di convalida viene eseguita sul lato client, da molto semplice. Gli attributi di convalida per le classi di modello server vengono propagati al client ed eseguiti automaticamente prima che il client contatta il server.

Esaminare il traffico di rete. Si noti che non vi sono Nessuna chiamata al server quando semplicissimo ha rilevato un errore. Ogni modifica valido ha restituito una richiesta POST per "/ attività/api/SaveChanges". Semplicissimo Raggruppa le modifiche e li invia insieme come una singola richiesta per il controller API Web `SaveChanges` metodo. È diverso dal modello di SPA KockoutJS, rendendo PUT, POST ed eliminare le richieste per ogni elemento singolarmente.

Si noti, inoltre, che non vi è alcun traffico di rete quando si passa tra l'elenco di attività e sulle pagine. Ciò avviene perché la query è stata vincolata alla cache locale molto semplice.

## <a name="peek-inside"></a>All'interno di anteprima

Questa applicazione, un lato client e sul lato server. Lo stack client-side è costituito un po' HTML e una combinazione di moduli di applicazione JavaScript (nella cartella "app") nonché le librerie JavaScript di terze parti (nella cartella "Scripts").

![](http://www.breezejs.com/sites/all/images/spa-template/NgClientArchitecture2.png)

L'architettura dell'interfaccia utente separa i widget HTML delle visualizzazioni dal codice di presentazione di supporto nel controller. Il sistema di associazione dati angolare coordinate visualizzazioni e controller in modo che ogni può svolgere la propria funzione senza una conoscenza approfondita di altro.

Il controller è richiesto il contesto dei dati per acquisire e salvare l'entità del modello. Il contesto dei dati delega la maggior parte del lavoro molto semplice, che costruisce oggetti modello rilevamento automatico dai risultati di query JSON.

Lo stack sul lato server è costituito da alcuni codice dello sviluppatore e tre le librerie .NET di principio: Breeze.NET API Web ed Entity Framework:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

L'architettura di base è la stessa del modello di SPA KockoutJS. Tuttavia, l'implementazione è molto più semplice: il DTO sono stati eliminati e la maggior parte dei dettagli di Entity Framework vengono delegata a Breeze.NET.

## <a name="next-steps"></a>Passaggi successivi

È consigliabile acquisire familiarità con il codice, basato sul [approfondite sulla](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa) di client e gli stack di server del sito Web molto semplice.

È possibile provare a giocare con la query sul lato client molto semplice. aggiungere alcuni filtri e gli ordinamenti. È possibile aggiungere altre proprietà del modello e altre entità a comprendere meglio per lo sviluppo di SPA end-to-end. Quando si è certi della progettazione, è possibile disinstallare le funzionalità di attività e sostituirli con valori personalizzati.

Buona codifica!
