---
uid: single-page-application/overview/templates/breezeangular-template
title: Modello Breeze/Angular | Microsoft Docs
author: madskristensen
description: Modello Breeze/Angular applicazione a pagina singola
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/08/2013
ms.topic: article
ms.assetid: db31e909-563a-4516-aadd-62aa210ac7e4
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/breezeangular-template
msc.type: authoredcontent
ms.openlocfilehash: 541d1a71b58a0d55651d823dc7425b7d1ce73cdb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378060"
---
<a name="breezeangular-template"></a>Modello Breeze/Angular
====================
da [Mads Kristensen](https://github.com/madskristensen)

> Il modello MVC Breeze/Angular è stato scritto da Ward Bell
> 
> [Scaricare il modello MVC Breeze/Angular](https://go.microsoft.com/fwlink/?LinkId=286437)


[AngularJS](http://angularjs.org) è una libreria open source da Google per la compilazione di applicazioni a pagina singola (SPAs). Offre l'associazione dati, inserimento di dipendenze e la gestione dello schermo. Combinarlo con [BreezeJS](http://www.breezejs.com/?utm_source=ms-spa), un'altra libreria open source per la modellazione dei dati e la gestione dei dati che dispone gli elementi essenziali per un'app client HTML/JavaScript eccezionale.

Modello Breeze/Angular SPA è una variante nel [modello Knockout. js SPA](../introduction/knockoutjs-template.md) incluso in ASP.NET e Web Tools 2012.2 Update. Se si ha Visual Studio, sarà necessario un esempio SPA in esecuzione in meno di 60 secondi.

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningTodoPage.png)

Verso l'esterno, l'applicazione di molto simile al modello di applicazione a singola pagina Knockout. js. Ma è piuttosto diverso dietro le quinte. Il modello Knockout. js Usa Knockout per l'associazione dati e AJAX non elaborato per l'accesso ai dati. Il modello Breeze/Angular Usa Angular per il data binding e Breeze per accedere ai dati. Queste librerie abilitare funzionalità aggiuntive, tra cui cronologia di navigazione tra le pagine.

Di seguito è una pagina di informazioni dell'applicazione:

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningAboutPage.png)

Questa pagina consente di visualizzare un log di eventi in esecuzione durante la sessione utente corrente, tra cui:

- Paging. Si noti la creazione del controller Todo in #2 e #7.
- Le query remote (n. 3) e le query della cache locale (& 7).
- Salvataggio di nuova (5, 6 #) e modificare le entità (4).
- Modifiche convalidate nel client (9), in modo che l'utente possa correggere eventuali errori prima di eseguire il commit delle modifiche al database.

Altre operazioni per esplorare in questo modello, tra cui:

- Caricamento dinamico dei modelli di visualizzazione HTML.
- Associazione di dati personalizzati tramite Angular "direttive".
- Inserimento di modularità e delle dipendenze.
- Eseguire query sui filtri, ordinamento, paging, proiezioni e inclusione delle entità correlate.
- Condivisione dei dati su più schermi.
- Salvataggio delle modifiche più come una singola transazione.
- Le regole di convalida propagate automaticamente dal server al client JavaScript.

Ma veniamo al dunque.

## <a name="create-a-breezeangular-template-project"></a>Creare un progetto di modello Breeze/Angular

Scaricare e installare il modello facendo clic sul pulsante Download precedente. Il modello viene fornito come un file di Visual Studio Extension (VSIX). Si potrebbe essere necessario riavviare Visual Studio.

Nel **modelli** riquadro, selezionare **modelli installati** ed espandere le **Visual c#** nodo. Sotto **Visual c#**, selezionare **Web**. Nell'elenco dei modelli di progetto, selezionare **applicazione Web ASP.NET MVC 4**. Denominare il progetto e fare clic su **OK**.

Nel **nuovo progetto** procedura guidata, selezionare **Breeze Angular SPA**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeNgSpaTemplate.png)

Premere CTRL+F5 per compilare ed eseguire l'applicazione senza eseguire il debug oppure premere F5 per eseguire il debug.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrLogin.png)

Quando l'applicazione viene eseguita prima di tutto, viene visualizzata una schermata di accesso. Fare clic sul collegamento "Iscrizione" e una nuova pagina glides nella visualizzazione, in cui è possibile immettere un nome utente e password. (L'account di accesso e registrazione pagine vengono compilate mediante ASP.NET MVC.) Quando si invia il modulo di registrazione, il server genera un TodoList con due voci per l'account. Quindi li presenta all'utente una nota giallo.

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

A questo punto si trova il terreno di SPA. Tutto ciò che viene visualizzata e verificarsi durante la modifica di elementi TODO viene eseguito il rendering e gestita sul client con l'aiuto di Knockout js e Breeze. Esplorare l'app come un utente... ma con attenzione dello sviluppatore. Usare gli strumenti di sviluppo nel browser per acquisire il traffico di rete. (In Internet Explorer: premere F12, selezionare la **Network** scheda e fare clic su **avvio dell'acquisizione**.) Prova ora quanto segue:

- Aggiungere un nuovo elemento Todo.
- Fare clic sull'etichetta e modificare il titolo dell'elemento Todo
- Selezionare una casella di controllo per contrassegnare l'elemento completato. Si noti che la casella di testo è disabilitato, in modo che il titolo non è più modificabile.
- Fare clic sulla "x" a destra dell'etichetta. L'elemento viene rimosso e viene eliminato dal database.
- Selezionare un altro elemento e deselezionare il titolo. Si otterrà un errore di convalida che il titolo è obbligatorio. Dopo una breve pausa, viene ripristinato il titolo precedente.
- Digitare un titolo estremamente lungo. Si otterrà un errore di convalida diverso che il titolo è troppo lungo.
- Fare clic sul pulsante "Aggiungi Todo List". Viene visualizzato un nuovo elenco a sinistra dell'elenco precedente.
- Provare a usare le convalide di lunghezza e il titolo del TodoList, attivare richiesto.
- Fare clic nella casella di testo del titolo per cancellare il messaggio di errore.
- Fare clic su "x" nel cerchio in alto a destra per eliminare il TodoList e relativi elementi TODO.
- Fare clic sul collegamento "About" in alto a destra per visualizzare una registrazione di queste attività.

La logica di convalida viene eseguita sul lato client, dal gioco da ragazzi. Gli attributi di convalida sulle classi del modello server siano propagati al client ed eseguiti automaticamente prima che il client contatta il server.

Esaminare il traffico di rete. Si noti che non erano Nessuna chiamata al server quando Breeze ha rilevato un errore. Ogni modifica valido ha restituito una richiesta POST per "/ api/Todo/SaveChanges". Gioco da ragazzi aggrega le modifiche e li invia insieme come una singola richiesta per il controller Web API `SaveChanges` (metodo). Che è diverso dal modello di applicazione a singola pagina KockoutJS, rendendo PUT, POST e DELETE singolarmente le richieste per ogni elemento.

Si noti, inoltre, che non vi è alcun traffico di rete quando si passa tra la TodoList e sulle pagine. Ciò avviene perché la query è stata vincolata alla cache locale Breeze.

## <a name="peek-inside"></a>Visualizza all'interno di

Questa applicazione ha un lato client e lato server. Lo stack client-side è costituito un piccolo HTML e una combinazione di moduli JavaScript dell'applicazione (nella cartella "app") nonché librerie JavaScript di terze parti (nella cartella "Scripts").

![](http://www.breezejs.com/sites/all/images/spa-template/NgClientArchitecture2.png)

L'architettura dell'interfaccia utente separa i widget HTML delle visualizzazioni dal codice di presentazione supporto nei controller. Il sistema di associazione dati Angular coordina le visualizzazioni e controller in modo che ogni possibile svolgere le proprie attività senza una conoscenza approfondita di altro.

Il controller richiede il contesto dei dati per acquisire e salvare l'entità del modello. Il contesto dati delega la maggior parte del lavoro Breeze, che costruisce oggetti con rilevamento automatico modello dai risultati di query JSON.

Lo stack di server-side è costituito da codice per sviluppatori e tre le librerie .NET di principio: Breeze.NET API Web ed Entity Framework:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

L'architettura di base è lo stesso come il modello di applicazione a singola pagina KockoutJS. Tuttavia, l'implementazione è molto più semplice: il DTO sono stati eliminati e la maggior parte dei dettagli Entity Framework vengono delegata a Breeze.NET.

## <a name="next-steps"></a>Passaggi successivi

È consigliabile esplorare il codice, interattiva per la [approfondite sulla](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa) del client e gli stack di server nel sito Web del gioco da ragazzi.

È possibile provare a giocare con la query lato client di gioco da ragazzi. aggiungere alcuni filtri e ordinamenti. È possibile aggiungere più entità per farsi un'idea migliore per lo sviluppo di SPA end-to-end e altre proprietà del modello. Quando si è certi della progettazione, è possibile eliminare le funzionalità di Todo e sostituirli con quelli personalizzati.

Buona codifica!
