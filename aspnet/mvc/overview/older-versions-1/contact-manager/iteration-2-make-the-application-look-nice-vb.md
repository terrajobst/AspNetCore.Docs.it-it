---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-vb
title: "Iterazione #2-migliorare l'applicazione aspetto interessante (VB) | Microsoft Docs"
author: microsoft
description: In questa iterazione è migliorare l'aspetto dell'applicazione modificando il valore predefinito di pagina master visualizzazione MVC ASP.NET e foglio di stile CSS.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: f65cb436-e493-46fd-9608-384b27385aa1
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-vb
msc.type: authoredcontent
ms.openlocfilehash: c1209a925a43bd7846a9dc07ce557c55bb1827ae
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381105"
---
<a name="iteration-2--make-the-application-look-nice-vb"></a>Iterazione #2-migliorare l'applicazione aspetto interessante (VB)
====================
da [Microsoft](https://github.com/microsoft)

[Scaricare il codice](iteration-2-make-the-application-look-nice-vb/_static/contactmanager_2_vb1.zip)

> In questa iterazione è migliorare l'aspetto dell'applicazione modificando il valore predefinito di pagina master visualizzazione MVC ASP.NET e foglio di stile CSS.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Creazione di un'applicazione di gestione dei contatti ASP.NET MVC (VB)
  

In questa serie di esercitazioni, creiamo un'intera applicazione di gestione dei contatti dall'inizio alla fine. L'applicazione Contact Manager consente di archiviare informazioni di contatto: nomi, indirizzi di posta elettronica e numeri di telefono: per un elenco di persone.

Si compilerà l'applicazione a varie iterazioni. A ogni iterazione, abbiamo migliorare gradualmente l'applicazione. L'obiettivo di questo approccio iterazione più è che consentono di comprendere il motivo per ogni modifica.

- Iterazione #1-creare l'applicazione. Nella prima iterazione, verranno create Contact Manager nel modo più semplice possibile. Viene aggiunto il supporto per le operazioni di base dei database: crea, lettura, aggiornamento ed eliminazione (CRUD).

- Iterazione #2: rendere l'applicazione l'aspetto. In questa iterazione è migliorare l'aspetto dell'applicazione modificando il valore predefinito di pagina master visualizzazione MVC ASP.NET e foglio di stile CSS.

- #3-aggiungere la convalida dei form dell'iterazione. Nella terza iterazione, aggiungiamo la convalida dei form di base. Persone è impedire l'invio di un modulo senza completare i campi del modulo richiesto. È anche convalidare gli indirizzi di posta elettronica e numeri di telefono.

- Iterazione #4-rendere l'applicazione a regime. In questa terza iterazione, possiamo usufruire dei diversi modelli di progettazione di software per renderne più semplice da gestire e modificare l'applicazione Contact Manager. Ad esempio, è eseguire il refactoring l'applicazione per usare il modello di Repository e il modello di inserimento delle dipendenze.

- Iterazione #5-creare gli unit test. Nell'iterazione del quinto, si semplifica l'applicazione di manutenzione e la modifica mediante l'aggiunta di unit test. È simulare le classi di modello di dati e generare unit test per i controller e logica di convalida.

- Iterazione #6-usare lo sviluppo basato su test. In questa iterazione sesta, viene aggiunto nuove funzionalità all'applicazione scrivendo unit test prima e scrivere codice sull'unit test. In questa iterazione, viene aggiunto gruppi di contatti.

- Iterazione #7-aggiungere funzionalità Ajax. Nell'iterazione del settimo, aggiungendo il supporto per Ajax è migliorare la velocità di risposta e le prestazioni dell'applicazione.

## <a name="this-iteration"></a>Questa iterazione

L'obiettivo di questa iterazione è migliorare l'aspetto dell'applicazione Contact Manager. Attualmente, Contact Manager usa la pagina master visualizzazione MVC ASP.NET predefinita e il foglio di stile CSS (vedere la figura 1). Questi don t sembrare scadenti, ma saprei want t Contact Manager per individuare esattamente come ogni altro sito Web ASP.NET MVC. Si desidera sostituire questi file con i file personalizzati.


[![La finestra di dialogo Nuovo progetto](iteration-2-make-the-application-look-nice-vb/_static/image1.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image1.png)

**Figura 01**: l'aspetto predefinito di un'applicazione ASP.NET MVC ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-2-make-the-application-look-nice-vb/_static/image2.png))


In questa iterazione, illustrato due approcci per migliorare la progettazione visiva dell'applicazione. In primo luogo, mostrerò come sfruttare la raccolta di schemi di MVC ASP.NET per scaricare un modello di progettazione di ASP.NET MVC gratuito. La raccolta di schemi di MVC ASP.NET consente di creare un'applicazione web professional senza svolgere alcuna operazione.

Ho deciso di non usare un modello dalla raccolta di progettazione MVC ASP.NET per l'applicazione Contact Manager. Al contrario, avevo un progetto personalizzato creato da una società di progettazione professionale. Nella seconda parte di questa esercitazione, spiegherò come ho collaborato con una società di progettazione professionale per creare il progetto finale di ASP.NET MVC.

## <a name="the-aspnet-mvc-design-gallery"></a>La raccolta di schemi di ASP.NET MVC

La raccolta di schemi di MVC ASP.NET è una risorsa gratuita fornita da Microsoft. La raccolta di MVC ASP.NET è disponibile all'indirizzo seguente:

[https://www.asp.net/mvc/gallery](https://www.asp.net/mvc/gallery)

La raccolta di schemi di MVC ASP.NET ospita una raccolta di progetti di sito Web gratuito che sono stati creati specificamente per l'uso in un progetto ASP.NET MVC. Le progettazioni vengono caricate dai membri della community. I visitatori alla raccolta possono votare per le progettazioni preferite (vedere la figura 2).


[![La finestra di dialogo Nuovo progetto](iteration-2-make-the-application-look-nice-vb/_static/image2.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image3.png)

**Figura 02**: la raccolta di schemi di ASP.NET MVC ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-2-make-the-application-look-nice-vb/_static/image4.png))


Mentre Scrivo questa esercitazione, la progettazione più diffusa in gallery è una progettazione denominata ottobre da David Hauser. È possibile usare questa struttura per un progetto ASP.NET MVC, completare i passaggi seguenti:

1. Scegliere il **scaricare** pulsante per scaricare il file October.zip nel computer.
2. Fare doppio clic su file October.zip scaricato e scegliere il **Unblock** pulsante (vedere la figura 3).
3. Decomprimere il file in una cartella denominata ottobre.
4. Selezionare tutti i file dalla cartella DesignTemplate contenuta nella cartella di ottobre, i file e scegliere l'opzione di menu **copia**.
5. Fare clic sul nodo del progetto ContactManager nella finestra Esplora soluzioni di Visual Studio e selezionare l'opzione di menu **Incolla** (vedere la figura 4).
6. Selezionare l'opzione di menu di Visual Studio **Edit, Trova e sostituisci, Sostituzione veloce** e sostituire *[NomeProgettoPersonale]* con *ContactManager* (vedere la figura 5).


[![La finestra di dialogo Nuovo progetto](iteration-2-make-the-application-look-nice-vb/_static/image3.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image5.png)

**Figura 03**: lo sblocco di un file scaricato dal web ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-2-make-the-application-look-nice-vb/_static/image6.png))


[![La finestra di dialogo Nuovo progetto](iteration-2-make-the-application-look-nice-vb/_static/image4.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image7.png)

**Figura 04**: la sovrascrittura dei file in Esplora soluzioni ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-2-make-the-application-look-nice-vb/_static/image8.png))


[![La finestra di dialogo Nuovo progetto](iteration-2-make-the-application-look-nice-vb/_static/image5.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image9.png)

**Figura 05**: la sostituzione [NomeProgetto] con ContactManager ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-2-make-the-application-look-nice-vb/_static/image10.png))


Dopo aver completato questi passaggi, l'applicazione web userà la nuova progettazione. La pagina nella figura 6 viene illustrato l'aspetto dell'applicazione Contact Manager con la progettazione di ottobre.


[![La finestra di dialogo Nuovo progetto](iteration-2-make-the-application-look-nice-vb/_static/image6.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image11.png)

**Figura 06**: ContactManager con il modello di ottobre ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-2-make-the-application-look-nice-vb/_static/image12.png))


## <a name="creating-a-custom-aspnet-mvc-design"></a>Creazione di una progettazione MVC ASP.NET personalizzati

La raccolta di schemi di ASP.NET MVC offre una selezione valida di stili design diversi. La raccolta fornisce un modo semplice per personalizzare l'aspetto delle applicazioni ASP.NET MVC. E, naturalmente, la raccolta è il grande vantaggio di essere completamente gratuito.

Tuttavia, si potrebbe essere necessario creare una struttura completamente univoca per il sito Web. In tal caso, è opportuno usare una società di progettazione di siti Web. Ho deciso di adottare questo approccio per la progettazione per l'applicazione Contact Manager.

Compresso di Contact Manager dall'iterazione 1 e inviato il progetto per la società di progettazione. Essi non dispone di Visual Studio (peccato su di essi!), che però t presentano un problema. È stato possibile scaricare gratuitamente Microsoft Visual Web Developer dal [ https://www.asp.net ](https://www.asp.net) sito Web e aprire l'applicazione Contact Manager in Visual Web Developer. In un paio di giorni, si ha prodotto la progettazione nella figura 7.


[![La finestra di dialogo Nuovo progetto](iteration-2-make-the-application-look-nice-vb/_static/image7.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image13.png)

**Figura 07**: la progettazione di ASP.NET MVC Contact Manager ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-2-make-the-application-look-nice-vb/_static/image14.png))


La nuova progettazione è costituita da due file principali: un nuovo file CSS e un nuovo file pagina master visualizzazione. Una pagina master visualizzazione include il layout e contenuto condiviso per le visualizzazioni in un'applicazione ASP.NET MVC. Ad esempio, la pagina master visualizzazione include l'intestazione, schede di navigazione e nel piè di pagina che vengono visualizzati nella figura 7. Hanno sovrascritto i Site. master visualizzazione pagina master esistente nella cartella Views\Shared con il nuovo file Site. master dell'azienda di progettazione,

La società di progettazione creato anche un nuovo foglio di stile CSS e un set di immagini. Ho inserito questi nuovi file nella cartella del contenuto e sovrascritto il file CSS esistente. È necessario inserire tutto il contenuto statico nella cartella del contenuto.

Si noti che la nuova progettazione di Contact Manager include le immagini per la modifica e l'eliminazione di contatti. Un'immagine di modifica ed eliminazione vengono visualizzati accanto a ogni contatto nella tabella HTML dei contatti.

In origine, questi collegamenti sono stati sottoposti a rendering con il codice HTML. Helper di ActionLink() simile al seguente:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-vb/samples/sample1.aspx)]

Il metodo Html.ActionLink() non supporta immagini (il metodo HTML codifica il testo del collegamento per motivi di sicurezza). Pertanto, sostituito le chiamate a Html.ActionLink() con chiamate a Url.Action() simile al seguente:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-vb/samples/sample2.aspx)]

Il metodo Html.ActionLink() esegue il rendering di un intero collegamento ipertestuale HTML. Il metodo Url.Action(), d'altra parte, viene eseguito il rendering solo l'URL senza il &lt;un&gt; tag.

Si noti, inoltre, che la nuova progettazione include sia selezionate e schede. Ad esempio, nella figura 8, il **Crea nuovo contatto** è selezionata della scheda e il **contatti personali** scheda non è selezionata.


[![La finestra di dialogo Nuovo progetto](iteration-2-make-the-application-look-nice-vb/_static/image8.jpg)](iteration-2-make-the-application-look-nice-vb/_static/image15.png)

**Figura 08**: selezionati e deselezionati schede ([fare clic per visualizzare l'immagine con dimensioni normali](iteration-2-make-the-application-look-nice-vb/_static/image16.png))


Per supportare il rendering sia selezionate e le schede, ho creato un helper HTML personalizzato denominato il MenuItemHelper. Questo metodo di supporto esegue il rendering di un &lt;li&gt; tag o un &lt;classe li = "selezionati"&gt; a seconda del fatto che il controller corrente e l'azione corrisponde al nome del controller e azione passato all'helper tag. Il codice per il MenuItemHelper è contenuto nel listato 1.

**Listato 1 – Helpers\MenuItemHelper.vb**

[!code-vb[Main](iteration-2-make-the-application-look-nice-vb/samples/sample3.vb)]

Il MenuItemHelper utilizza internamente la classe TagBuilder per compilare il &lt;li&gt; tag HTML. La classe TagBuilder è una classe di utilità molto utile che è possibile usare ogni volta che è necessario creare un nuovo tag HTML. Include metodi per l'aggiunta di attributi, aggiunta di classi CSS, la generazione di ID e la modifica il tag s HTML interno.

## <a name="summary"></a>Riepilogo

In questa iterazione è stata migliorata la progettazione visiva dell'applicazione ASP.NET MVC. Prima di tutto sono stati introdotti per la raccolta di schemi di MVC ASP.NET. Si è appreso come scaricare i modelli di progettazione libera dalla raccolta di schemi di MVC ASP.NET che è possibile usare nelle applicazioni ASP.NET MVC.

Successivamente, abbiamo parlato di come è possibile creare un progetto personalizzato modificando il file del foglio di stile CSS predefiniti e il file di pagina di visualizzazione master. Per supportare la nuova progettazione, è stato necessario apportare alcune piccole modifiche all'applicazione Contact Manager. Ad esempio, abbiamo aggiunto un nuovo helper HTML denominato MenuItemHelper che vengono visualizzate le schede selezionate e.

Nell'iterazione successiva, abbiamo affrontare l'oggetto molto importante della convalida. È consigliabile aggiungere codice di convalida all'applicazione in modo che un utente non è possibile creare un nuovo contatto senza fornire valori richiesti, ad esempio una persona s prima di tutto nome e cognome.

> [!div class="step-by-step"]
> [Precedente](iteration-1-create-the-application-vb.md)
> [Successivo](iteration-3-add-form-validation-vb.md)
