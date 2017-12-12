---
uid: single-page-application/overview/templates/breezeknockout-template
title: Modello molto semplice/Knockout | Documenti Microsoft
author: madskristensen
description: Modello di applicazione a pagina singola semplicissimo/Knockout
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 3bd94827-3c59-448f-abc3-36e6df4858db
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/breezeknockout-template
msc.type: authoredcontent
ms.openlocfilehash: 07ec099a0381458fe42c1972a2554f76fd34638c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="breezeknockout-template"></a>Modello molto semplice/Knockout
====================
da [Mads Kristensen](https://github.com/madskristensen)

> Il modello MVC semplicissimo/Knockout è stato scritto dal Ward Bell
> 
> [Scaricare il modello MVC semplicissimo/Knockout](https://go.microsoft.com/fwlink/?LinkId=282649)


Si è sentito parlare di "applicazione a pagina singola" (SPA) e spiega che cos'è. Mentre è possibile leggere su di esso, si potrebbe invece Provalo adesso. Ma che è necessario scaricare un esempio? Se si dispone di Visual Studio, è necessario un esempio SPA e in esecuzione in meno di 60 secondi con ASP.NET MVC 4 "semplicissimo/Knockout singola pagina modello dell'applicazione".

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

## <a name="what-is-the-breezeknockout-spa-template"></a>Che cos'è il modello di SPA semplicissimo/Knockout?

La maggior parte dei modelli di progetto di generano uno scheletro di applicazione. Archiviare carne su tali ossa aggiungendo il codice e infine distribuire un'applicazione funzionante. Il modello molto semplice/Knockout SPA è diverso. Genera un'applicazione di esempio per poter esaminare. Viene illustrato come una progettazione di applicazioni di SPA e molte delle tecniche per la creazione di un SPA.

Il modello molto semplice/Knockout è una variante di [modello Knockout.js SPA](../introduction/knockoutjs-template.md) incluso nell'aggiornamento 2012.2 di strumenti Web di ASP.NET. Il modello molto semplice SPA genera un'applicazione con la stessa esperienza utente, ma dispone di un'implementazione diversa, l'utilizzo di molto semplice per la gestione di dati.

Il modello di SPA Knockout.js effettua le richieste di servizio con jQuery raw AJAX, che è adatto a un'applicazione semplice. Ma più sofisticata App hanno requisiti di gestione di dati più complessi. Ad esempio, la maggior parte delle applicazioni:

- Eseguire una query e query nuovamente sul server durante una sessione utente estesa.
- Aggiungere filtri di query, ordinamento e paging.
- Condividono gli stessi dati in più schermate.
- Continue modifiche a molti oggetti, quindi salvarli come una singola transazione.
- Convalida le modifiche nel client, pertanto l'utente possa correggere eventuali errori prima di eseguire il commit delle modifiche al database.

La libreria BreezeJS gestisce queste routine per l'utente, che per sviluppare l'applicazione logica e l'esperienza utente più importanti.

[**Semplicissimo** ](http://www.breezejs.com/?utm_source=ms-spa) è una libreria open source per la compilazione di applicazioni di dati complessi in JavaScript e HTML, i tipi di App che sono stati recapitati in passato come applicazioni desktop autonome.

Il modello molto semplice/Knockout contribuisce di utilizzare tale primo importante passo avanti verso un'infrastruttura di gestione di dati più affidabile. Genera un'applicazione di esempio Todo esternamente identica al modello di SPA Knockout.js. All'interno, sostituisce il livello di dati AJAX con molto semplice, pertanto è possibile confrontare i due approcci side-by-side. Ovviamente, appena tocca il potenziale di un'applicazione molto semplice. Ma vedrai funzionamento molto semplice e quanto è necessario eseguire la transizione.

Ma veniamo al dunque.

## <a name="create-a-breezeknockout-template-project"></a>Creare un progetto di modello molto semplice/Knockout

Scaricare e installare il modello fare clic sul pulsante Download. Il modello viene fornito come un file di Visual Studio Extension (VSIX). Si potrebbe essere necessario riavviare Visual Studio.

Nel **modelli** riquadro, selezionare **modelli installati** ed espandere il **Visual c#** nodo. In **Visual c#**selezionare **Web**. Nell'elenco dei modelli di progetto, selezionare **applicazione Web ASP.NET MVC 4**. Denominare il progetto e fare clic su **OK**.

Nel **nuovo progetto** procedura guidata, selezionare **semplicissimo Knockout SPA**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeKOSpaTemplate.png)

Premere Ctrl + F5 per compilare ed eseguire l'applicazione senza debug oppure premere F5 per eseguire il debug.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

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

La logica di convalida viene eseguita sul lato client, da molto semplice. Gli attributi di convalida per le classi di modello server vengono propagati al client ed eseguiti automaticamente prima che il client contatta il server.

Esaminare il traffico di rete. Si noti che non vi sono Nessuna chiamata al server quando semplicissimo ha rilevato un errore. Ogni modifica valido ha restituito una richiesta POST per "/ attività/api/SaveChanges". Semplicissimo Raggruppa le modifiche e li invia insieme come una singola richiesta per il controller API Web `SaveChanges` metodo. È diverso dal modello di SPA KockoutJS, rendendo PUT, POST ed eliminare le richieste per ogni elemento singolarmente.

## <a name="peek-inside"></a>All'interno di anteprima

Questa applicazione, un lato client e sul lato server. Lo stack client-side è costituito un po' HTML e una combinazione di moduli di applicazione JavaScript (nella cartella "app") nonché le librerie JavaScript di terze parti (nella cartella "Scripts").

![](http://www.breezejs.com/sites/all/images/spa-template/ClientArchitecture.png)

Se è stato esaminato il modello di SPA Knockout.js, questo dovrebbe essere molto familiare. Concentrarsi sulle caselle blu. L'architettura dell'interfaccia utente è Model-View-ViewModel (MVVM), in cui i widget HTML della visualizzazione sono nettamente separati dal codice di presentazione di supporto nel modello di visualizzazione. Un sistema di associazione dati (in questo caso, Knockout) coordina la visualizzazione e il modello di visualizzazione in modo che ogni può svolgere la propria funzione senza una conoscenza approfondita di altro.

Il modello incapsula i dati di attività. Entità nel modello vengono create aggiungendo molto semplice con una proprietà osservabile Knockout, pertanto possono essere associate direttamente a widget nella vista. Il modello di visualizzazione chiede il contesto dei dati per acquisire e salvare l'entità del modello. Il contesto dei dati delega la maggior parte del lavoro molto semplice.

Lo stack sul lato server è costituito da alcuni codice dello sviluppatore e tre le librerie .NET di principio: Breeze.NET API Web ed Entity Framework:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

L'architettura di base è la stessa del modello di SPA KockoutJS. Tuttavia, l'implementazione è molto più semplice: il DTO sono stati eliminati e la maggior parte dei dettagli di Entity Framework vengono delegata a Breeze.NET.

## <a name="next-steps"></a>Passaggi successivi

È consigliabile acquisire familiarità con il codice, basato sul [approfondite sulla](http://www.breezejs.com/spa-template?utm_source=ms-spa) di client e gli stack di server del sito Web molto semplice.

È possibile provare a giocare con la query sul lato client molto semplice. aggiungere alcuni filtri e gli ordinamenti. È possibile aggiungere altre proprietà del modello e altre entità a comprendere meglio per lo sviluppo di SPA end-to-end. Quando si è certi della progettazione, è possibile disinstallare le funzionalità di attività e sostituirli con valori personalizzati.

Non appena sarà pronto per il passaggio successivo big: aggiunta di schermate del client e lo spostamento tra di essi. Verranno lasciate questo modello SPA e attivare a uno stack di SPA più completo, ad esempio [degli asciugamani a caldo di John Papa](https://github.com/johnpapa/HotTowel#readme "degli asciugamani Hot"), che aggiunge Durandal alla combinazione di molto semplice e Knockout.
