---
uid: mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
title: "Iterazione #2: rendere l'applicazione nice (c#) | Documenti Microsoft"
author: microsoft
description: In questa iterazione, è migliorare l'aspetto dell'applicazione modificando il valore predefinito di pagina master di visualizzazione ASP.NET MVC e foglio di stile CSS.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: f1173feb-11ee-4017-8f3f-86599ea6ae13
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-2-make-the-application-look-nice-cs
msc.type: authoredcontent
ms.openlocfilehash: cad28fb6ff02625674e59674d1ec08d52373c269
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="iteration-2--make-the-application-look-nice-c"></a>Iterazione #2: rendere l'applicazione nice (c#)
====================
by [Microsoft](https://github.com/microsoft)

[Scaricare il codice](iteration-2-make-the-application-look-nice-cs/_static/contactmanager_2_cs1.zip)

> In questa iterazione, è migliorare l'aspetto dell'applicazione modificando il valore predefinito di pagina master di visualizzazione ASP.NET MVC e foglio di stile CSS.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Creazione di un'applicazione ASP.NET MVC di gestione dei contatti (c#)
  

In questa serie di esercitazioni, si compila un'intera applicazione di gestione dei contatti dall'inizio completamento. L'applicazione Contact Manager consente di archiviare le informazioni di contatto - nomi, i numeri di telefono e indirizzi di posta elettronica: per un elenco di persone.

È compilare l'applicazione più iterazioni. A ogni iterazione, gradualmente è migliorare l'applicazione. L'obiettivo di questo approccio iterazione più è che consentono di comprendere il motivo per ogni modifica.

- Iterazione #1 - creare l'applicazione. Nella prima iterazione, verranno create Contact Manager in modo più semplice possibile. Viene aggiunto il supporto per le operazioni di database basic: creazione, lettura, aggiornamento ed eliminazione (CRUD).

- Iterazione #2 - verificare l'applicazione l'aspetto. In questa iterazione, è migliorare l'aspetto dell'applicazione modificando il valore predefinito di pagina master di visualizzazione ASP.NET MVC e foglio di stile CSS.

- Iterazione #3 - aggiungere la convalida dei form. Nella terza iterazione, è aggiungere la convalida di form di base. È impedire agli utenti di inviare un modulo senza completare i campi modulo necessari. È inoltre possibile convalidare gli indirizzi di posta elettronica e numeri di telefono.

- Iterazione #4: verificare l'applicazione ad accoppiamento debole. In questa terza iterazione, possiamo usufruire dei diversi modelli di progettazione di software per renderne più semplice gestire e modificare l'applicazione Gestione contatti. Ad esempio, si effettua il refactoring l'applicazione di utilizzare il modello di Repository e il modello di inserimento di dipendenze.

- Iterazione #5 - creare unit test. Nella quinta iterazione, si rende l'applicazione di più facile da gestire e modificare tramite l'aggiunta di unit test. È simulare il nostro classi del modello di dati e generare unit test per i controller e logica di convalida.

- Iterazione 6 # - utilizzare sviluppo basato su test. In questa iterazione sesto è aggiungere nuove funzionalità per l'applicazione scrivendo unit test prima e la scrittura di codice per gli unit test. In questa iterazione, è aggiungere gruppi di contatti.

- Iterazione #7 - aggiunta di funzionalità Ajax. Nella settima iterazione, è migliorare la velocità di risposta e prestazioni dell'applicazione aggiunta del supporto per Ajax.

## <a name="this-iteration"></a>Questa iterazione

L'obiettivo di questa iterazione è per migliorare l'aspetto dell'applicazione Contact Manager. Attualmente, il responsabile del contatto utilizza la pagina master di visualizzazione ASP.NET MVC predefinita e il foglio di stile CSS (vedere la figura 1). Questi don t aspetto non valida, ma è possibile eliminare desidera t Contact Manager solo come ogni altro sito Web ASP.NET MVC. Si desidera sostituire questi file con file personalizzati.


[![La finestra di dialogo Nuovo progetto](iteration-2-make-the-application-look-nice-cs/_static/image1.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image1.png)

**Figura 01**: l'aspetto predefinito di un'applicazione MVC ASP.NET ([fare clic per visualizzare l'immagine ingrandita](iteration-2-make-the-application-look-nice-cs/_static/image2.png))


In questa iterazione, illustrerò due approcci per migliorare la progettazione visiva dell'applicazione. In primo luogo, viene illustrato come sfruttare la raccolta di schemi di ASP.NET MVC per scaricare un modello di progettazione di ASP.NET MVC gratuito. La raccolta di schemi di ASP.NET MVC consente di creare un'applicazione web professionale senza svolgere alcuna operazione.

Ho deciso di non utilizzare un modello dalla raccolta schemi di MVC ASP.NET per l'applicazione Gestione contatti. È stato invece di un progetto personalizzato creato da una società di progettazione professionale. Nella seconda parte di questa esercitazione, spiegano come ho collaborato con una società di progettazione professionale per creare il progetto ASP.NET MVC finale.

## <a name="the-aspnet-mvc-design-gallery"></a>La raccolta di schemi di MVC ASP.NET

La raccolta di schemi di MVC ASP.NET è una risorsa gratuita fornita da Microsoft. La raccolta di MVC ASP.NET è disponibile all'indirizzo seguente:

[https://www.asp.net/mvc/gallery](https://www.asp.net/mvc/gallery)

La raccolta di schemi di MVC ASP.NET ospita un insieme di strutture di sito Web gratuito che sono stati creati in modo specifico per l'utilizzo in un progetto ASP.NET MVC. Schemi vengono caricati dai membri della community. Visitatori della raccolta possono votare per i progetti Preferiti (vedere la figura 2).


[![La finestra di dialogo Nuovo progetto](iteration-2-make-the-application-look-nice-cs/_static/image2.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image3.png)

**Figura 02**: la raccolta di schemi di ASP.NET MVC ([fare clic per visualizzare l'immagine ingrandita](iteration-2-make-the-application-look-nice-cs/_static/image4.png))


In questa esercitazione, la progettazione più diffuso nella raccolta è un progetto denominato ottobre da David Hauser. È possibile utilizzare questa struttura per un progetto ASP.NET MVC, completare i passaggi seguenti:

1. Fare clic su di **scaricare** per scaricare il file October.zip nel computer.
2. Il file October.zip scaricato e fare clic su di **Unblock** pulsante (vedere la figura 3).
3. Decomprimere il file in una cartella denominata ottobre.
4. Selezionare tutti i file nella cartella DesignTemplate contenuta nella cartella di ottobre, fare clic sui file e selezionare l'opzione di menu **copia**.
5. Il nodo del progetto ContactManager nella finestra Esplora soluzioni di Visual Studio e scegliere l'opzione di menu **Incolla** (vedere la figura 4).
6. Selezionare l'opzione di menu di Visual Studio **modifica, Trova e sostituisci, Sostituzione veloce** e sostituire *[NomeProgettoPersonale]* con *ContactManager* (vedere Figura 5).


[![La finestra di dialogo Nuovo progetto](iteration-2-make-the-application-look-nice-cs/_static/image3.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image5.png)

**Figura 03**: lo sblocco di un file scaricato dal web ([fare clic per visualizzare l'immagine ingrandita](iteration-2-make-the-application-look-nice-cs/_static/image6.png))


[![La finestra di dialogo Nuovo progetto](iteration-2-make-the-application-look-nice-cs/_static/image4.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image7.png)

**Figura 04**: sovrascrivendo i file in Esplora soluzioni ([fare clic per visualizzare l'immagine ingrandita](iteration-2-make-the-application-look-nice-cs/_static/image8.png))


[![La finestra di dialogo Nuovo progetto](iteration-2-make-the-application-look-nice-cs/_static/image5.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image9.png)

**Figura 05**: sostituire [NomeProgetto] con ContactManager ([fare clic per visualizzare l'immagine ingrandita](iteration-2-make-the-application-look-nice-cs/_static/image10.png))


Dopo aver completato questi passaggi, l'applicazione web utilizzerà la nuova progettazione. La pagina nella figura 6 illustra l'aspetto dell'applicazione responsabile contatto con la progettazione di ottobre.


[![La finestra di dialogo Nuovo progetto](iteration-2-make-the-application-look-nice-cs/_static/image6.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image11.png)

**Figura 06**: ContactManager con il modello di ottobre ([fare clic per visualizzare l'immagine ingrandita](iteration-2-make-the-application-look-nice-cs/_static/image12.png))


## <a name="creating-a-custom-aspnet-mvc-design"></a>Creazione di una progettazione MVC ASP.NET personalizzati

La raccolta di schemi di MVC ASP.NET è una buona selezione degli stili di progettazione diverse. La raccolta fornisce un modo semplice per personalizzare l'aspetto delle applicazioni ASP.NET MVC. E, naturalmente, la raccolta è il grande vantaggio di essere completamente gratuita.

Tuttavia, occorre creare una struttura completamente univoca per il sito Web. In tal caso, è opportuno utilizzare una società di progettazione del sito Web. Ho deciso di adottare questo approccio per la progettazione per l'applicazione Gestione contatti.

Compresso il contatto Manager dall'iterazione 1 e inviato al progetto per la società di progettazione. Essi non possiede Visual Studio (peccato su di essi!), ma che non presentano un problema. Fossero in grado di scaricare Microsoft Visual Web Developer gratuitamente dal [ https://www.asp.net ](https://www.asp.net) sito Web e aprire l'applicazione Gestione contatti in Visual Web Developer. In un paio di giorni, sono era prodotti progettazione nella figura 7.


[![La finestra di dialogo Nuovo progetto](iteration-2-make-the-application-look-nice-cs/_static/image7.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image13.png)

**Figura 07**: la progettazione di ASP.NET MVC Contact Manager ([fare clic per visualizzare l'immagine ingrandita](iteration-2-make-the-application-look-nice-cs/_static/image14.png))


La nuova progettazione è costituita da due file principali: un nuovo file CSS e un nuovo file pagina master visualizzazione. Una pagina master visualizzazione contiene il layout e il contenuto condiviso per le visualizzazioni in un'applicazione MVC ASP.NET. Ad esempio, la pagina master visualizzazione include l'intestazione, le schede di navigazione e nel piè di pagina vengono visualizzati nella figura 7. Hanno sovrascritto i Site. master visualizzazione pagina master esistente nella cartella Views\Shared con il nuovo file Site. master dell'azienda, progettazione

La società di progettazione viene creata anche un nuovo foglio di stile CSS e set di immagini. Ho inseriti i nuovi file nella cartella del contenuto e sovrascritto il file Site.css esistente. Nella cartella del contenuto, è consigliabile inserire tutto il contenuto statico.

Si noti che la nuova progettazione per la gestione di contatto sono incluse immagini per la modifica e l'eliminazione di contatti. Un'immagine di modifica ed eliminazione vengono visualizzati accanto a ogni contatto nella tabella HTML di contatti.

In origine, questi collegamenti sono stati sottoposti a rendering con il codice HTML. Supporto ActionLink() simile al seguente:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample1.aspx)]

Il metodo Html.ActionLink() non supporta immagini (il metodo HTML codifica il testo del collegamento per motivi di sicurezza). Pertanto, le chiamate a Html.ActionLink() I sostituito con chiamate a Url.Action() simile al seguente:

[!code-aspx[Main](iteration-2-make-the-application-look-nice-cs/samples/sample2.aspx)]

Il metodo Html.ActionLink() esegue il rendering di un collegamento ipertestuale HTML intero. Il metodo Url.Action(), d'altra parte, viene eseguito il rendering solo l'URL senza il &lt;un&gt; tag.

Si noti inoltre che la nuova progettazione include sia selezionate e schede. Ad esempio, nella figura 8, il **creare nuovo contatto** scheda sia selezionata e **contatti personali** scheda non è selezionata.


[![La finestra di dialogo Nuovo progetto](iteration-2-make-the-application-look-nice-cs/_static/image8.jpg)](iteration-2-make-the-application-look-nice-cs/_static/image15.png)

**Figura 08**: selezionati e deselezionati schede ([fare clic per visualizzare l'immagine ingrandita](iteration-2-make-the-application-look-nice-cs/_static/image16.png))


Per supportare il rendering sia selezionate e le schede, è stato creato un helper HTML personalizzato denominato il MenuItemHelper. Questo metodo di supporto esegue il rendering di un &lt;li&gt; tag o una &lt;classe li = "selezionato"&gt; tag a seconda se il controller corrente e l'azione corrispondono per il nome di azione e del controller passato al supporto. Il codice per il MenuItemHelper è contenuto in elenco 1.

**Elenco 1 - Helpers\MenuItemHelper.cs**

[!code-csharp[Main](iteration-2-make-the-application-look-nice-cs/samples/sample3.cs)]

Il MenuItemHelper utilizza internamente la classe TagBuilder per compilare il &lt;li&gt; tag HTML. La classe TagBuilder è una classe di utilità molto utile che è possibile utilizzare ogni volta che si desidera creare un nuovo tag HTML. Include metodi per l'aggiunta di attributi, aggiunta di classi CSS, la generazione di ID e modificando il tag s HTML interno.

## <a name="summary"></a>Riepilogo

In questa iterazione, è migliorato la progettazione visiva dell'applicazione ASP.NET MVC. In primo luogo, sono stati introdotti per la raccolta di schemi di MVC ASP.NET. È stato descritto come scaricare i modelli di progettazione disponibile dalla raccolta schemi di MVC ASP.NET che è possibile utilizzare nelle applicazioni ASP.NET MVC.

Successivamente, è descritto come creare un progetto personalizzato modificando il file del foglio di stile CSS predefinito e il file di pagina di visualizzazione master. Per supportare la nuova progettazione, è stato necessario apportare alcune piccole modifiche per l'applicazione di gestione di contatto. Ad esempio, è aggiunto un nuovo helper HTML, denominato MenuItemHelper che vengono visualizzate le schede selezionate e deselezionate.

Nell'iterazione successiva, è affrontare oggetto molto importante della convalida. È consigliabile aggiungere codice di convalida per l'applicazione in modo che un utente non è possibile creare un nuovo contatto senza fornire i valori richiesti, ad esempio una persona s innanzitutto e del cognome.

> [!div class="step-by-step"]
> [Precedente](iteration-1-create-the-application-cs.md)
> [Successivo](iteration-3-add-form-validation-cs.md)
