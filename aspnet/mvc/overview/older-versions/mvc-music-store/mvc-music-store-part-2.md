---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Parte 2: Controller | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Parte 2 illustra i controller.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: a854feff675cae302b9927d209808257b7d087cb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381803"
---
<a name="part-2-controllers"></a>Parte 2: controller
====================
da [Jon Galloway](https://github.com/jongalloway)

> Store la musica MVC è un'applicazione di esercitazione introduce e dettagliata spiega come usare ASP.NET MVC e Visual Studio per lo sviluppo web.  
>   
> La Store musica MVC è un'implementazione di archivio di esempio leggero che vende gli album musicali online e ne implementa amministrazione sito di base, accesso dell'utente e la funzionalità di carrello acquisti.  
>   
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Parte 2 illustra i controller.


Framework web tradizionali, gli URL in ingresso vengono in genere mappati ai file su disco. Ad esempio: una richiesta per un URL, ad esempio "/ Products. aspx" o "/ Products" potrebbe essere elaborata da un file "Products" o "Products".

Framework MVC basati su Web eseguire il mapping degli URL al codice lato server in modo leggermente diverso. Anziché eseguire il mapping degli URL in ingresso ai file, ma eseguono il mapping degli URL ai metodi nelle classi. Queste classi vengono chiamate "Controller" e sono responsabili per l'elaborazione delle richieste HTTP in ingresso, gestione dell'input dell'utente, eseguire il recupero e salvataggio dei dati e determinare la risposta da inviare al client (visualizzazione del contenuto HTML, scaricare un file, il reindirizzamento a un altro URL e così via).

## <a name="adding-a-homecontroller"></a>Aggiunta di una classe HomeController

Inizieremo la nostra applicazione MVC Music Store mediante l'aggiunta di una classe Controller che gestirà l'URL alla Home page del sito. Illustreremo seguono le convenzioni di denominazione predefinito di MVC ASP.NET e chiamarla HomeController.

Fare doppio clic nella cartella "Controller" all'interno di Esplora soluzioni e selezionare "Aggiungi", quindi il comando "Controller in corso":

![](mvc-music-store-part-2/_static/image1.jpg)

Verrà visualizzata la finestra di dialogo "Aggiungi Controller". Denominare il controller "HomeController" e fare clic su Aggiungi.

![](mvc-music-store-part-2/_static/image1.png)

Si creerà un nuovo file HomeController.cs, con il codice seguente:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

Per avviare semplicemente possibile, è possibile sostituire il metodo di indice con un metodo semplice che restituisce solo una stringa. Saranno apportate due modifiche:

- Modificare il metodo per restituire una stringa anziché un ActionResult
- Modificare l'istruzione return per restituire "Hello da casa"

Il metodo dovrebbe ora essere simile al seguente:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a>Esecuzione dell'applicazione

A questo punto è possibile eseguire il sito. È possibile avviare il server web e provare a utilizzare il sito usando quanto segue:

- Scegliere la voce di menu Avvia debug ⇨ di Debug
- Fare clic sul pulsante freccia verde sulla barra degli strumenti ![](mvc-music-store-part-2/_static/image2.jpg)
- Usare i tasti di scelta rapida F5.

Usando uno dei passaggi precedenti verrà compilare il progetto e quindi causare il Server di sviluppo ASP.NET che viene incorporato in Visual Web Developer per iniziare. Una notifica verrà visualizzata nell'angolo inferiore della schermata per indicare che ha avviato il Server di sviluppo ASP.NET e verrà indicato il numero di porta che viene eseguito sotto.

![](mvc-music-store-part-2/_static/image2.png)

Visual Web Developer quindi aprirà automaticamente una finestra del browser con URL che punta al nostro server web. Questo ci consentirà di provare rapidamente l'applicazione web:

![](mvc-music-store-part-2/_static/image3.png)

Bene, questa è stata abbastanza rapida: è stato creato un nuovo sito Web, aggiunta una funzione tre inline e abbiamo testo in un browser. Non stare scienza, ma è un inizio.

*Nota: Visual Web Developer include il Server di sviluppo ASP.NET, che verrà eseguito il sito Web su un numero casuale gratuito "port". Nella schermata precedente, il sito viene eseguito in `http://localhost:26641/`, pertanto utilizza porta 26641. Il numero di porta sarà diverso. Quando si parla /Store/Browse like dell'URL in questa esercitazione, che risulteranno dopo il numero di porta. Supponendo che un numero di porta di 26641, passare a/Store/Sfoglia implica esplorando `http://localhost:26641/Store/Browse`.*

## <a name="adding-a-storecontroller"></a>Aggiunta di un StoreController

È stato aggiunto un oggetto semplice che implementa la Home Page del sito. Questo punto, aggiungere un altro controller che verranno utilizzate per implementare la funzionalità di esplorazione dell'archivio file musicali. Il controller di archiviazione offrirà supporto per tre scenari:

- Una pagina di presentazione di generi musica nel nostro store musica
- Una pagina di esplorazione che elenca tutti i album musicali in genere particolare
- Una pagina che contiene informazioni su un album musicali specifici

Si inizierà aggiungendo una nuova classe StoreController.. Se hai già fatto, interrompere l'esecuzione dell'applicazione la chiusura del browser oppure selezionando la voce di menu arresta debug ⇨ di Debug.

A questo punto aggiungere un nuovo StoreController. Come abbiamo fatto con HomeController, faremo questo facendo clic sulla cartella "Controller" all'interno di Esplora soluzioni e scegliendo Aggiungi -&gt;voce di menu Controller

![](mvc-music-store-part-2/_static/image4.png)

Nostro nuovo StoreController dispone già di un metodo "Index". Si userà questo metodo "Index" per implementare la pagina di presentazione che elenca tutti i generi nel nostro store musica. Viene anche aggiunto altri due metodi per implementare i due altri scenari vogliamo nostro StoreController gestire: esplorazione e i dettagli.

Questi metodi (Index, Sfoglia e Details) nel Controller vengono chiamati "Azioni del Controller" e come abbiamo già visto con il metodo di azione HomeController.Index (), il loro lavoro consiste nel rispondere alle richieste di URL e (in genere) determinare il tipo di contenuto devono essere inviati nuovamente al browser o all'utente che ha richiamato l'URL.

Si inizierà la nostra implementazione StoreController modificando theIndex() metodo per restituire la stringa "Hello da Store.Index()" e aggiungeremo metodi simili per Browse e Details():

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

Eseguire nuovamente il progetto e passare gli URL seguenti:

- / Store
- / Store/esplorazione
- / Store/dettagli

L'accesso a questi URL verrà richiamare i metodi di azione all'interno di Controller e restituire le risposte di stringa:

![](mvc-music-store-part-2/_static/image5.png)

Bene, ma si tratta di stringhe solo costanti. È possibile renderli dinamico, in modo accettare informazioni dall'URL e li visualizzi nell'output della pagina.

Prima di tutto si modificherà il metodo di azione di esplorazione per recuperare un valore di stringa di query dall'URL. È possibile farlo mediante l'aggiunta di un parametro "genre" per il metodo di azione. Quando si esegue questa operazione, ASP.NET MVC passerà automaticamente i parametri post qualsiasi stringa di query o un modulo denominati "genre" per il metodo di azione quando viene richiamato.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

*Nota: Viene usato il metodo di utilità HttpUtility per purificare l'input dell'utente. In questo modo si impedisce agli utenti di inserimento di Javascript in visualizzazione con un collegamento, ad esempio /Store/Browse? Genre =&lt;script&gt;Window. Location = 'http://hackersite.com'&lt;/script&gt;.*

A questo punto è possibile passare a/Store/Sfoglia? Genre = Disco

![](mvc-music-store-part-2/_static/image6.png)

Modifichiamo quindi l'azione Details per leggere e visualizzare un parametro di input denominato ID. A differenza dei nostri metodo precedente, abbiamo non incorporamento il valore ID come parametro di stringa di query. È invece sarà incorporarlo direttamente all'interno dell'URL stesso. Ad esempio: /Store/Details/5.

ASP.NET MVC consente di eseguire facilmente questa operazione senza la necessità di alcuna configurazione. Convenzione di routing predefinito di ASP.NET MVC consiste nel considerare il segmento di URL dopo il nome del metodo di azione come un parametro denominato "ID". Se il metodo di azione ha un parametro denominato ID quindi ASP.NET MVC automaticamente passerà il segmento URL all'utente come parametro.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

Eseguire l'applicazione e passare a /Store/Details/5:

![](mvc-music-store-part-2/_static/image7.png)

È utile esaminare quanto è stato fatto finora:

- Abbiamo creato un nuovo progetto ASP.NET MVC in Visual Web Developer
- È stata illustrata la struttura di cartelle di base di un'applicazione ASP.NET MVC
- È stato appreso come eseguire il nostro sito Web utilizzando ASP.NET Development Server
- Abbiamo creato due classi Controller: una classe HomeController e un StoreController
- Sono stati aggiunti i metodi di azione per il controller che rispondono alle richieste di URL e restituiscono testo al browser


> [!div class="step-by-step"]
> [Precedente](mvc-music-store-part-1.md)
> [Successivo](mvc-music-store-part-3.md)
