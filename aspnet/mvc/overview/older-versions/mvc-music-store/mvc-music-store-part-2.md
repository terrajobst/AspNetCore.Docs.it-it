---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Parte 2: Controller | Documenti Microsoft'
author: jongalloway
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio. Parte 2 vengono illustrati i controller.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: bdafd751e996e759d516d0fa25b09eff21241ed7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="part-2-controllers"></a>Parte 2: controller
====================
da [Jon Galloway](https://github.com/jongalloway)

> L'archivio di musica MVC è un'applicazione di esercitazione che vengono presentati e dettagliate sull'utilizzo di MVC ASP.NET e Visual Studio per lo sviluppo web.  
>   
> L'archivio di musica MVC è un'implementazione dell'archivio di esempio semplice che vende album musicali online e ne implementa amministrazione sito di base, account utente e funzionalità di carrello acquisti.  
>   
> Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio. Parte 2 vengono illustrati i controller.


Con il framework di web tradizionali, gli URL in ingresso vengono in genere associati ai file su disco. Ad esempio: una richiesta per un URL come "/ Products" o "/ Products.php" possono essere elaborate da un file "Products" o "Products.php".

Framework MVC basata sul Web di eseguire il mapping degli URL al codice server in modo leggermente diverso. Anziché eseguire il mapping degli URL in ingresso ai file, gli URL sono invece associati ai metodi nelle classi. Queste classi vengono chiamate "Controller" e sono responsabili per l'elaborazione delle richieste HTTP in ingresso, gestione dell'input dell'utente, il recupero e il salvataggio dei dati e determinare la risposta da inviare al client (visualizzazione del contenuto HTML, scaricare un file, il reindirizzamento a un altro URL e così via).

## <a name="adding-a-homecontroller"></a>Aggiunta di un HomeController

L'applicazione MVC negozio inizieremo aggiungendo una classe Controller che gestirà gli URL per la Home page del sito. Viene seguono le convenzioni di denominazione predefinito di ASP.NET MVC e chiamarlo HomeController.

Nella cartella "Controller" all'interno di Esplora soluzioni e scegliere "Aggiungi", quindi il "Controller..." comando:

![](mvc-music-store-part-2/_static/image1.jpg)

Verrà visualizzata la finestra di dialogo "Aggiungi Controller". Denominare il controller "HomeController" e fare clic sul pulsante Aggiungi.

![](mvc-music-store-part-2/_static/image1.png)

Si creerà un nuovo file HomeController. cs, con il codice seguente:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

Per avviare semplicemente come possibile, sostituire il metodo di indice con un metodo semplice che restituisce una stringa. Due modifiche da apportare:

- Modificare il metodo per restituire una stringa anziché un ActionResult
- Modificare l'istruzione return per restituire "Hello dalla pagina iniziale"

Il metodo dovrebbe risultare simile al seguente:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a>Esecuzione dell'applicazione

Ora si esegue il sito. È possibile avviare il server web e provare a utilizzare il sito usando uno dei seguenti:

- Scegliere la voce di menu Avvia debug ⇨ di Debug
- Fare clic sul pulsante freccia verde nella barra degli strumenti![](mvc-music-store-part-2/_static/image2.jpg)
- Utilizzare i tasti di scelta rapida F5.

Utilizzando uno dei passaggi precedenti verrà compilare il progetto e causare quindi il Server di sviluppo ASP.NET che viene compilato in Visual Web Developer per avviare. Una notifica verrà visualizzato nell'angolo inferiore della schermata per indicare che ha avviato il Server di sviluppo ASP.NET e verrà visualizzato il numero di porta che viene eseguito con.

![](mvc-music-store-part-2/_static/image2.png)

Visual Web Developer verrà quindi aperto automaticamente una finestra del browser con URL punta al server web. Ciò permetterà di provare rapidamente l'applicazione web:

![](mvc-music-store-part-2/_static/image3.png)

OK, che è stata abbastanza rapida: è stato creato un nuovo sito Web, aggiungere una funzione grafico three line e abbiamo testo in un browser. Non stare informatica, ma è un modo per iniziare.

*Nota: Visual Web Developer comprende il Server di sviluppo ASP.NET che eseguiranno il sito Web in un numero casuale libero "porta". Nella schermata precedente, il sito è in esecuzione in `http://localhost:26641/`, pertanto è usando porta 26641. Il numero di porta è diverso. Quando si parlerà like /Store/Browse dell'URL in questa esercitazione, che entra dopo il numero di porta. Presupponendo un numero di porta di 26641, la selezione di archivio/Sfoglia implica la selezione di `http://localhost:26641/Store/Browse`.*

## <a name="adding-a-storecontroller"></a>Aggiunta di un StoreController

È stato aggiunto un semplice HomeController che implementa la Home Page del sito. A questo punto aggiungere un altro controller che verranno utilizzate per implementare la funzionalità di esplorazione del Negozio musica. Il controller di archivio verrà supportano tre scenari:

- Una pagina di elenco di generi di musica nel negozio musica
- Una pagina di esplorazione che elenca tutti gli album musica in un particolare genere
- Una pagina di dettagli che contiene informazioni su un album musicali specifici

Inizieremo aggiungendo una nuova classe StoreController... Se hai già fatto, arrestare l'esecuzione dell'applicazione mediante la chiusura del browser o selezionando il ⇨ Termina debug dal menu Debug.

Aggiungere un nuovo StoreController. Proprio come abbiamo fatto con HomeController, faremo questo facendo clic sulla cartella "Controller" all'interno di Esplora soluzioni e scegliendo Aggiungi -&gt;voce di menu Controller

![](mvc-music-store-part-2/_static/image4.png)

Il nuovo StoreController dispone già di un metodo "Index". Questo metodo "Index" useremo per implementare la pagina di elenco in cui sono elencati tutti i generi negozio musica. Inoltre, verranno aggiunte due metodi aggiuntivi per implementare i due altri scenari vogliamo nostri StoreController di gestire: Sfoglia e dettagli.

Questi metodi (indice, esplorazione e i dettagli) all'interno di questo Controller vengono chiamati "Azioni del Controller" e come è stato già illustrato con il metodo di azione HomeController.Index (), il processo è per rispondere alle richieste di URL e (in genere) determinare il tipo di contenuto deve essere inviato nuovamente il browser o l'utente che ha richiamato l'URL.

Si inizierà l'implementazione di StoreController modificando theIndex() metodo per restituire la stringa "Hello da Store.Index()" e verrà aggiunto metodi simili per Browse () e Details():

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

Eseguire nuovamente il progetto e passare i seguenti URL:

- / Store
- / Archivio/Sfoglia
- / / Dettagli archivio

L'accesso a questi URL verrà richiamare i metodi di azione all'interno di questo Controller e restituire risposte stringa:

![](mvc-music-store-part-2/_static/image5.png)

Che è molto utile, ma questi sono stringhe solo costanti. Di seguito renderli dinamici, in modo che ricevono informazioni dall'URL e visualizzarla nell'output della pagina.

Innanzitutto si modificherà il metodo di azione di esplorazione per recuperare un valore di stringa di query dell'URL. È possibile farlo mediante l'aggiunta di un parametro "genre" per il metodo di azione. Quando si esegue questa operazione, ASP.NET MVC passerà automaticamente eventuali parametri post querystring o modulo denominati "genre" per il metodo di azione quando viene richiamata.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

*Nota: Si usa il metodo di utilità HttpUtility per purificare l'input dell'utente. Questo impedisce agli utenti di Javascript inserendo la visualizzazione con un collegamento come /Store/Browse? Genre =&lt;script&gt;window.location= 'http://hackersite.com'&lt;/script&gt;.*

Ora consente di passare a archivio/Sfoglia? Genre = Disco

![](mvc-music-store-part-2/_static/image6.png)

Modificare quindi l'azione di dettagli per leggere e visualizzare un parametro di input denominato ID. A differenza di questo metodo precedente, è non incorporare il valore ID come parametro di stringa di query. È invece sarà incorporarlo direttamente nell'URL stesso. Ad esempio: /Store/Details/5.

ASP.NET MVC consente di farlo facilmente senza dover configurare alcuna operazione. Convenzione di routing predefinita di ASP.NET MVC consiste nel considerare il segmento di un URL dopo il nome del metodo di azione come un parametro denominato "ID". Se il metodo di azione ha un parametro denominato ID quindi ASP.NET MVC automaticamente passerà il segmento URL all'utente come parametro.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

Eseguire l'applicazione e passare a /Store/Details/5:

![](mvc-music-store-part-2/_static/image7.png)

Riepilogo si abbiamo finora:

- Abbiamo creato un nuovo progetto ASP.NET MVC in Visual Web Developer
- È stata illustrata la struttura di cartelle di base di un'applicazione MVC ASP.NET
- È stato spiegato come eseguire il sito Web utilizzando il Server di sviluppo ASP.NET
- Abbiamo creato due classi Controller: un HomeController e un StoreController
- Sono stati aggiunti i metodi di azione per il controller che risponde alle richieste di URL e restituisce testo nel browser


>[!div class="step-by-step"]
[Precedente](mvc-music-store-part-1.md)
[Successivo](mvc-music-store-part-3.md)
