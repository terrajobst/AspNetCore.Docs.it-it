---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
title: Aggiunta di una vista | Documenti Microsoft
author: shanselman
description: Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Creare un'applicazione web semplice la lettura e scrittura da un database.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: e8f1515c-c277-47ff-a23e-224118f13f02
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
msc.type: authoredcontent
ms.openlocfilehash: 978d7980274c072ed559b54ed69ab86245b6c5a7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/10/2018
ms.locfileid: "30869595"
---
<a name="adding-a-view"></a>Aggiunta di una vista
====================
da [Scott Hanselman](https://github.com/shanselman)

> Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Si creerà un'applicazione web semplice la lettura e scrittura da un database. Visitare il [area risorse di ASP.NET MVC](../../../index.md) per trovare altri ASP.NET MVC, esercitazioni ed esempi.


In questa sezione verrà esaminata la possibilità di installare la classe HelloWorldController utilizzare un file di modello di visualizzazione per incapsulano la generazione di risposte HTML a un client.

Iniziamo con un modello di visualizzazione con metodo di indice. Questo metodo viene chiamato indice ed è nel HelloWorldController. Attualmente, il metodo Index () restituisce una stringa con un messaggio che è codificato all'interno della classe Controller.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample1.cs)]

Modificare ora il metodo di indice invece simile al seguente:

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample2.cs)]

Aggiungere ora un modello di visualizzazione per il progetto che è possibile utilizzare per il metodo Index (). A tale scopo, fare clic con il mouse in un punto al centro il metodo di indice e fare clic su Aggiungi visualizzazione...

![immagine](getting-started-with-mvc-part3/_static/image1.png)

Verrà visualizzata la finestra di dialogo "Aggiungi visualizzazione" che offre alcune opzioni per la modalità con cui si vuole creare un modello di visualizzazione che può essere utilizzato dal metodo di indice. Per il momento, non apporta alcuna modifica e fare clic sul pulsante Aggiungi.

[![Visualizza finestra di dialogo Aggiungi](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)

Dopo aver fatto clic Aggiungi una nuova cartella e un nuovo file verrà visualizzato nella cartella della soluzione, come illustrato di seguito. Ora si dispone di una cartella HelloWorld in viste e un file Index.aspx all'interno di tale cartella.

[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)

Il nuovo file di indice è già aperto e pronto per la modifica. Aggiungere il testo sotto il primo &lt;h2&gt;indice&lt;/h2&gt; come "Hello World".

[!code-html[Main](getting-started-with-mvc-part3/samples/sample3.html)]

Eseguire l'applicazione e visitare [ `http://localhost:xx/HelloWorld` ](http://localhostxx) nuovamente nel browser. Il metodo di indice nel nostro controller in questo esempio non è stata eseguita alcuna operazione, ma ha chiamato "View() restituito" indica che si desidera utilizzare un file di modello di visualizzazione per il rendering di una risposta al client. Poiché non è stato specificato il nome del file modello di visualizzazione da utilizzare in modo esplicito, ASP.NET MVC specificata utilizzando il file di visualizzazione Index.aspx all'interno della cartella \Views\HelloWorld. È ora possibile visualizzare la stringa che è codificato nella visualizzazione.

[![Index - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)

È buona. Si noti tuttavia che il titolo del Browser è indicato "Index" e il titolo di grandi dimensioni nella pagina è indicato "Applicazione MVC." È necessario modificare quelle.

### <a name="changing-views-and-master-pages"></a>Modifica delle viste e le pagine Master

In primo luogo, è necessario modificare il testo "Applicazione MVC." Tale testo è condivisa e verrà visualizzato in ogni pagina. In realtà solo in una posizione nel codice, anche se è presente in ogni pagina nell'applicazione. Passare alla cartella /Views/Shared in Esplora soluzioni e aprire il file Site. master. Questo file è denominato a una pagina Master ed è condivisa "shell" che utilizzano tutte le altre pagine.

Si noti che afferma ContentPlaceholder "MainContent" in questo file.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample4.aspx)]

Il segnaposto è in tutte le pagine create verranno visualizzati, nella pagina master "wrapping". Provare a modificare il titolo, quindi eseguire l'app e accedere a più pagine. Si noterà che la modifica di uno venga visualizzata in più pagine.

[!code-html[Main](getting-started-with-mvc-part3/samples/sample5.html)]

È ora ogni pagina disporrà di intestazione principale - H1 - di "Film applicazione MVC." Che gestisce il testo bianco nella parte superiore sono condivisi tra tutte le pagine.

Ecco Site interamente con la modifica del titolo:

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample6.aspx)]

A questo punto, è necessario modificare il titolo della pagina di indice.

Aprire /HelloWorld/Index.aspx. Sono due posizioni da modificare. In primo luogo, che viene visualizzato il titolo nel titolo del browser, quindi l'intestazione secondaria - che è anche H2. Provare a impostare ognuna leggermente diversa in modo da visualizzare il frammento di codice modifica la parte dell'app.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample7.aspx)]

Eseguire l'app e visitare /Movies. Si noti che il titolo del browser, l'intestazione primaria e le intestazioni di secondari sono stati modificati. È facile apportare modifiche big in un'app con piccole modifiche alla propria visualizzazione.

[![Elenco di film - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)

Il minimo di "dati" (in questo caso "Hello World!" messaggio) è stato disco anche se codificati. Abbiamo V (Views) e abbiamo C (controller), ma ancora Nessun M (modello). A breve verrà esaminato come creare un database e recuperare i dati del modello.

## <a name="passing-a-viewmodel"></a>Il passaggio di un elemento ViewModel

Prima di passare a un database e descrivere i modelli, tuttavia, esaminiamo innanzitutto "ViewModel". Si tratta di oggetti che rappresentano i requisiti di un modello di visualizzazione per il rendering di una risposta HTML a un client. Sono in genere vengono creati e passato da una classe Controller per un modello di visualizzazione e deve contenere solo i dati richiesti, il modello di visualizzazione e non più.

In precedenza con l'esempio HelloWorld, metodo di azione Welcome () ha richiesto un nome e un parametro numTimes e visualizzarli nel browser. Per evitare che il Controller di continuare a eseguire il rendering la risposta direttamente, verifichiamo invece una classe per contenere i dati e quindi passarlo un modello di visualizzazione per il rendering nuovamente la risposta HTML utilizzarlo. In questo modo è interessato il Controller con un aspetto e il modello di visualizzazione un'altra: consentono di mantenere pulita "separazione delle problematiche" all'interno dell'applicazione.

Tornare al file HelloWorldController.cs e aggiungere una nuova classe "WelcomeViewModel" e modificare il metodo iniziale all'interno del controller. Di seguito è riportato il HelloWorldController.cs completo con la nuova classe nello stesso file.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample8.cs)]

Anche se è su più righe, metodo di completamento dell'installazione è piuttosto semplice solo due istruzioni del codice. La prima istruzione pacchetti i due parametri in un oggetto ViewModel e il secondo passa l'oggetto risultante nella visualizzazione.

A questo punto è necessario un modello di visualizzazione iniziale. Fare clic con il metodo di completamento dell'installazione e selezionare Aggiungi visualizzazione. Questa volta, si verrà controllo "Crea visualizzazione fortemente tipizzata" e selezionare la classe WelcomeViewModel dall'elenco a discesa. Questa nuova vista conosceranno solo WelcomeViewModels e non altri tipi di oggetti.

> *Nota: È necessario avere compilato una volta dopo l'aggiunta di WelcomeViewModel per visualizzato nell'elenco a discesa.*


Ecco la finestra di dialogo Aggiungi visualizzazione quale deve essere simile. Fare clic sul pulsante Aggiungi. ![Aggiungere che un cerchio di visualizzazione](getting-started-with-mvc-part3/_static/image10.png)

Aggiungere questo codice sotto il &lt;h2&gt; in Welcome.aspx il nuovo. Verrà verificare un ciclo ed ecco le volte che l'utente indicato che si deve!

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample9.aspx)]

Si noti inoltre mentre si digita che perché è detto visualizzazione relativi il WelcomeViewModel (sono sposate, ricordare?) che è possibile ottenere Intellisense utile ogni volta che si fa riferimento l'oggetto modello come illustrato nella schermata seguente:

[![Codice sorgente NumTime](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)

Eseguire l'applicazione e visitare `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` nuovamente. Dopo l'acquisizione di dati dall'URL, viene passato automaticamente a questo Controller, il Controller pacchetti backup dei dati in un elemento ViewModel e passa l'oggetto sulla visualizzazione. La vista che consente di visualizzare i dati in formato HTML per l'utente.

[![Pagina iniziale - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)

Che è un tipo di una "M" per il modello, ma non il tipo di database. Prendiamo elementi appreso e creare un database di film.

> [!div class="step-by-step"]
> [Precedente](getting-started-with-mvc-part2.md)
> [Successivo](getting-started-with-mvc-part4.md)
