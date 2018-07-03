---
uid: single-page-application/overview/templates/breezeknockout-template
title: Modello Breeze/Knockout | Microsoft Docs
author: madskristensen
description: Modello Breeze/Knockout applicazione a pagina singola
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 3bd94827-3c59-448f-abc3-36e6df4858db
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/breezeknockout-template
msc.type: authoredcontent
ms.openlocfilehash: 48ee0463fe950c28832523986a2242417411c96a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376864"
---
<a name="breezeknockout-template"></a>Modello Breeze/Knockout
====================
da [Mads Kristensen](https://github.com/madskristensen)

> Modello Breeze/Knockout MVC è stato scritto da Ward Bell
> 
> [Scaricare il modello MVC Breeze/Knockout](https://go.microsoft.com/fwlink/?LinkId=282649)


Si è sentito parlare di "applicazione a pagina singola" (SPA) e chiesto quale sia. Anche se è stato possibile leggere su di esso, si sarebbe piuttosto Provalo adesso. Ma chi può ora scaricare un esempio? Se si ha Visual Studio, è necessario un esempio di applicazione a singola pagina e in esecuzione in meno di 60 secondi con ASP.NET MVC 4 modello "Breeze/Knockout applicazione a pagina singola".

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

## <a name="what-is-the-breezeknockout-spa-template"></a>Che cos'è il modello di applicazione a singola pagina Breeze/Knockout?

La maggior parte dei modelli di progetto di generano uno scheletro di applicazione. È inserire pelle nuda su tali ossa aggiungendo il codice e infine distribuire un'applicazione funzionante. Modello Breeze/Knockout SPA è diverso. Genera un'applicazione di esempio per poter esaminare. Viene illustrato come una progettazione di applicazioni SPA e molte delle tecniche per la creazione di un'applicazione a singola pagina.

Modello Breeze/Knockout è una variante nel [modello Knockout. js SPA](../introduction/knockoutjs-template.md) incluso in ASP.NET e Web Tools 2012.2 Update. Il modello Breeze SPA genera un'applicazione con la stessa esperienza utente, ma presenta un'altra implementazione, utilizzo gioco da ragazzi per la gestione dei dati.

Il modello Knockout. js SPA effettua le richieste di servizio con jQuery non elaborati AJAX, ovvero adeguata per una semplice applicazione. Ma le app più sofisticate hanno requisiti di gestione dati più complessi. Ad esempio, la maggior parte delle applicazioni:

- Eseguire query e ripetere la query il server durante una sessione utente estesa.
- Aggiungere filtri di query, ordinamento e paging.
- Condividere gli stessi dati su più schermi.
- Continue modifiche a molti oggetti, quindi salvarle come una singola transazione.
- Convalidare le modifiche sul client, in modo che l'utente possa correggere eventuali errori prima di eseguire il commit delle modifiche al database.

La libreria BreezeJS gestisce queste attività per l'utente, liberando per sviluppare l'applicazione per la logica e l'esperienza utente più importanti.

[**Gioco da ragazzi** ](http://www.breezejs.com/?utm_source=ms-spa) è una libreria open source per la compilazione di applicazioni di dati avanzati in JavaScript e HTML, i tipi di App che in passato sono stati recapitati come applicazioni desktop autonome.

Modello Breeze/Knockout consente di eseguire tale primo importante passo avanti verso un'infrastruttura di gestione dati più affidabile. Produce un'applicazione Todo di esempio esternamente identica al modello di applicazione a singola pagina Knockout. js. All'interno, sostituisce il livello di dati AJAX con Breeze, pertanto è possibile confrontare i due approcci side-by-side. Naturalmente, appena tocca il potenziale di un'applicazione di gioco da ragazzi. Ma si apprenderà come Breeze funziona e come poco è necessario per rendere tale transizione.

Ma veniamo al dunque.

## <a name="create-a-breezeknockout-template-project"></a>Creare un progetto di modello Breeze/Knockout

Scaricare e installare il modello facendo clic sul pulsante Download precedente. Il modello viene fornito come un file di Visual Studio Extension (VSIX). Si potrebbe essere necessario riavviare Visual Studio.

Nel **modelli** riquadro, selezionare **modelli installati** ed espandere le **Visual c#** nodo. Sotto **Visual c#**, selezionare **Web**. Nell'elenco dei modelli di progetto, selezionare **applicazione Web ASP.NET MVC 4**. Denominare il progetto e fare clic su **OK**.

Nel **nuovo progetto** procedura guidata, selezionare **Breeze Knockout SPA**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeKOSpaTemplate.png)

Premere CTRL+F5 per compilare ed eseguire l'applicazione senza eseguire il debug oppure premere F5 per eseguire il debug.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

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

La logica di convalida viene eseguita sul lato client, dal gioco da ragazzi. Gli attributi di convalida sulle classi del modello server siano propagati al client ed eseguiti automaticamente prima che il client contatta il server.

Esaminare il traffico di rete. Si noti che non erano Nessuna chiamata al server quando Breeze ha rilevato un errore. Ogni modifica valido ha restituito una richiesta POST per "/ api/Todo/SaveChanges". Gioco da ragazzi aggrega le modifiche e li invia insieme come una singola richiesta per il controller Web API `SaveChanges` (metodo). Che è diverso dal modello di applicazione a singola pagina KockoutJS, rendendo PUT, POST e DELETE singolarmente le richieste per ogni elemento.

## <a name="peek-inside"></a>Visualizza all'interno di

Questa applicazione ha un lato client e lato server. Lo stack client-side è costituito un piccolo HTML e una combinazione di moduli JavaScript dell'applicazione (nella cartella "app") nonché librerie JavaScript di terze parti (nella cartella "Scripts").

![](http://www.breezejs.com/sites/all/images/spa-template/ClientArchitecture.png)

Se è stato esaminato il modello di applicazione a singola pagina Knockout. js, questa deve avere un aspetto familiare. Concentrare le caselle blu. L'architettura dell'interfaccia utente è Model-View-ViewModel (MVVM), in cui i widget HTML della visualizzazione sono nettamente separati dal codice di presentazione di supporto nel modello di visualizzazione. Un sistema di associazione dati (in questo caso, Knockout) coordina la visualizzazione e il modello di visualizzazione in modo che ogni possibile svolgere le proprie attività senza una conoscenza approfondita di altro.

Il modello incapsula i dati di attività. Le entità del modello vengono costruite con gioco da ragazzi con le proprietà osservabili Knockout, in modo che possano essere associate direttamente ai widget nella visualizzazione. Il modello di visualizzazione richiede il contesto dei dati per acquisire e salvare l'entità del modello. Il contesto dati delega la maggior parte del lavoro Breeze.

Lo stack di server-side è costituito da codice per sviluppatori e tre le librerie .NET di principio: Breeze.NET API Web ed Entity Framework:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

L'architettura di base è lo stesso come il modello di applicazione a singola pagina KockoutJS. Tuttavia, l'implementazione è molto più semplice: il DTO sono stati eliminati e la maggior parte dei dettagli Entity Framework vengono delegata a Breeze.NET.

## <a name="next-steps"></a>Passaggi successivi

È consigliabile esplorare il codice, interattiva per la [approfondite sulla](http://www.breezejs.com/spa-template?utm_source=ms-spa) del client e gli stack di server nel sito Web del gioco da ragazzi.

È possibile provare a giocare con la query lato client di gioco da ragazzi. aggiungere alcuni filtri e ordinamenti. È possibile aggiungere più entità per farsi un'idea migliore per lo sviluppo di SPA end-to-end e altre proprietà del modello. Quando si è certi della progettazione, è possibile eliminare le funzionalità di Todo e sostituirli con quelli personalizzati.

Presto sarà pronto per il passaggio successivo di big data: aggiunta di nuove schermate del client e spostarsi tra di essi. Si sarà tralasciare questo modello di applicazione a singola pagina e attiva a uno stack di SPA più completo, ad esempio [John Papa Hot Towel](https://github.com/johnpapa/HotTowel#readme "Hot Towel"), che aggiunge Durandal alla combinazione di gioco da ragazzi e Knockout.
