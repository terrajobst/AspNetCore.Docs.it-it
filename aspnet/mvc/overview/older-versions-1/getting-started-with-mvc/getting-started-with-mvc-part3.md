---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
title: Aggiunta di una vista | Microsoft Docs
author: shanselman
description: Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Creare un'applicazione web semplice che legge e scrive da un database.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: e8f1515c-c277-47ff-a23e-224118f13f02
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part3
msc.type: authoredcontent
ms.openlocfilehash: e3b923aa5572781261c5a75546700faf223703d4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834504"
---
<a name="adding-a-view"></a>Aggiunta di una vista
====================
da [Scott Hanselman](https://github.com/shanselman)

> Si tratta di un'esercitazione per principianti che introduce i concetti di base di ASP.NET MVC. Si creerà una semplice applicazione web che legge e scrive da un database. Visitare il [centro di formazione di ASP.NET MVC](../../../index.md) per trovare altri ASP.NET MVC, esercitazioni ed esempi.


In questa sezione si intende esaminare come possiamo nostra classe HelloWorldController utilizzare un file di modello di visualizzazione per incapsulare correttamente la generazione di risposte HTML a un client.

Iniziamo con un modello di visualizzazione con il metodo di indice. Il metodo viene chiamato indice ed è nell'HelloWorldController. Attualmente il metodo Index () restituisce una stringa con un messaggio hardcoded nella classe Controller.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample1.cs)]

Modifichiamo ora il metodo Index deve invece essere come segue:

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample2.cs)]

Questo punto, aggiungere un modello di visualizzazione per il progetto che è possibile usare per il metodo Index (). A tale scopo, fare doppio clic con il puntatore del mouse in una posizione al centro il metodo di indice e fare clic su Aggiungi visualizzazione...

![immagine](getting-started-with-mvc-part3/_static/image1.png)

Verrà visualizzata la finestra di dialogo "Aggiungi visualizzazione" ci fornisce alcune opzioni per il modo in cui si vuole creare un modello di visualizzazione che può essere utilizzato dal metodo di indice. Per il momento, non apportare alcuna modifica e fare clic sul pulsante Aggiungi.

[![Visualizza finestra di dialogo Aggiungi](getting-started-with-mvc-part3/_static/image3.png)](getting-started-with-mvc-part3/_static/image2.png)

Dopo che si fa clic su Aggiungi, una nuova cartella e un nuovo file verranno visualizzati nella cartella della soluzione, come illustrato di seguito. Ora è disponibile una cartella HelloWorld in viste e un file index. aspx all'interno della cartella.

[![solutionexplorerwithindex](getting-started-with-mvc-part3/_static/image5.png)](getting-started-with-mvc-part3/_static/image4.png)

Il nuovo file di indice è già aperta e pronta per la modifica. Aggiungere il testo sotto il primo &lt;h2&gt;indice&lt;/h2&gt; , ad esempio "Hello World".

[!code-html[Main](getting-started-with-mvc-part3/samples/sample3.html)]

Eseguire l'applicazione e visitare [ `http://localhost:xx/HelloWorld` ](http://localhostxx) nuovamente nel browser. Il metodo di indice nel controller in questo esempio non effettuata alcuna operazione, ma è stata eseguita la chiamata "View() restituito" indicato che si desidera utilizzare un file di modello di visualizzazione per il rendering di una risposta al client. Perché non si ha specificato il nome del file del modello di visualizzazione da usare in modo esplicito, per impostazione predefinita MVC ASP.NET usando il file index. aspx della visualizzazione all'interno della cartella \Views\HelloWorld. A questo punto viene visualizzata la stringa che è hardcoded nella nostra vista.

[![Index - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image7.png)](getting-started-with-mvc-part3/_static/image6.png)

Sembra abbastanza positivo. Si noti tuttavia che il titolo del Browser afferma "Index", il titolo della pagina big dice "Applicazione MVC." È possibile modificare quelle.

### <a name="changing-views-and-master-pages"></a>Modifica delle viste e le pagine Master

In primo luogo, è possibile modificare il testo "Applicazione MVC." Tale testo viene condivisa e viene visualizzato in ogni pagina. Viene effettivamente visualizzato in un'unica posizione nel codice, anche se è in ogni pagina nell'app. Passare alla cartella /Views/Shared in Esplora soluzioni e aprire il file Site. master. Questo file viene chiamato una pagina Master ed è condivisa "shell" che utilizzano tutte le altre pagine.

Si noti che un testo con la dicitura ContentPlaceholder "MainContent" in questo file.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample4.aspx)]

Tale segnaposto è dove tutte le pagine create verranno visualizzate "wrapping" nella pagina master. Provare a modificare il titolo, quindi eseguire l'app e visitare più pagine. Si noterà che la modifica di uno venga visualizzata in più pagine.

[!code-html[Main](getting-started-with-mvc-part3/samples/sample5.html)]

A questo punto ogni pagina avrà l'intestazione primaria - ovvero H1 - di "My MVC Movie applicazione". Che gestisce il testo bianco nella parte superiore sono condivisi tra tutte le pagine.

Ecco il Site. master nella sua interezza con il titolo modificato:

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample6.aspx)]

A questo punto, è possibile modificare il titolo della pagina di indice.

Aprire /HelloWorld/Index.aspx. Ecco due modi per modificare. In primo luogo, il titolo che viene visualizzato nel titolo del browser, quindi l'intestazione secondaria - ovvero H2 - anche. Renderà ciascuno leggermente diverso, è possibile visualizzare il frammento di codice viene modificato quale parte dell'app.

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample7.aspx)]

Eseguire l'app e visitare /Movies. Si noti che il titolo del browser, l'intestazione primaria e le intestazioni secondarie sono cambiati. È facile apportare cambiamenti nella tua app con piccole modifiche alla visualizzazione.

[![Elenco di film - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image9.png)](getting-started-with-mvc-part3/_static/image8.png)

Il numero minimo di "data" (in questo caso "Hello World!" messaggio) era difficile anche se codificati. Abbiamo V (viste) e abbiamo C (controller), ma ancora alcun M (modello). A breve si verrà illustrato come creare un database e recuperare i dati del modello da quest'ultimo.

## <a name="passing-a-viewmodel"></a>Il passaggio di un elemento ViewModel

Prima di passare a un database e comunicare con informazioni sui modelli, tuttavia, esaminiamo innanzitutto "ViewModel". Si tratta di oggetti che rappresentano novità richiede un modello di vista per eseguire il rendering di una risposta HTML a un client. Essi vengono in genere creati passata da una classe Controller da un modello di vista e deve contenere solo i dati che richiede il modello di vista - e non oltre.

In precedenza con l'esempio HelloWorld, il metodo di azione Welcome () ha richiesto un nome e un parametro numTimes e restituirlo al browser. Invece di ottenere il Controller di continuare a eseguire il rendering di questa risposta direttamente, creiamo invece una classe piccola per contenere i dati e quindi passarlo un modello di vista per eseguire il rendering nuovamente la risposta HTML utilizzarlo. In questo modo il Controller si occupa di un aspetto e il modello di vista altro – ci permette di mantenere pulita "separazione delle problematiche" all'interno dell'applicazione.

Tornare al file HelloWorldController.cs e aggiungere una nuova classe "WelcomeViewModel" e modificare il metodo di completamento dell'installazione all'interno del controller. Ecco il HelloWorldController.cs completa con la nuova classe nello stesso file.

[!code-csharp[Main](getting-started-with-mvc-part3/samples/sample8.cs)]

Anche se è su più righe, il metodo iniziale è davvero solo due istruzioni del codice. La prima istruzione in pacchetti i due parametri in un oggetto ViewModel e la seconda passa l'oggetto risultante alla visualizzazione.

A questo punto è necessario un modello di vista Welcome! Fare clic con il pulsante destro del metodo iniziale e selezionare Aggiungi visualizzazione. Questa volta, si sarà controllare "Crea una visualizzazione fortemente tipizzata" e selezionare la classe WelcomeViewModel dall'elenco a discesa. Questa nuova visualizzazione verrà solo a conoscenza WelcomeViewModels e non altri tipi di oggetti.

> *Nota: È necessario avere compilato una sola volta dopo l'aggiunta di WelcomeViewModel per venga visualizzato nell'elenco a discesa.*


Ecco come dovrebbe risultare la finestra di dialogo Aggiungi visualizzazione. Fare clic sul pulsante Aggiungi. ![Aggiungere che un cerchio di visualizzazione](getting-started-with-mvc-part3/_static/image10.png)

Aggiungere questo codice sotto la &lt;h2&gt; in Welcome.aspx di nuovo. Illustreremo apportare un ciclo e dal benvenuto come tutte le volte che l'utente indicato che si deve!

[!code-aspx[Main](getting-started-with-mvc-part3/samples/sample9.aspx)]

Inoltre, si noti che mentre si digita che poiché l'avevamo detto visualizzazione relativi il WelcomeViewModel (sono sposate, ricordare?) che viene visualizzato Intellisense utile ogni volta che si fa riferimento l'oggetto modello come illustra lo screenshot seguente:

[![Codice sorgente NumTime](getting-started-with-mvc-part3/_static/image12.png)](getting-started-with-mvc-part3/_static/image11.png)

Eseguire l'applicazione e visitare `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` nuovamente. A questo punto è in corso il reindirizzamento dei dati dall'URL, viene passato automaticamente nel Controller, il Controller in pacchetti i dati in un elemento ViewModel e passa tale oggetto nella visualizzazione. La vista che consente di visualizzare i dati in formato HTML all'utente.

[![Attività iniziali - Windows Internet Explorer](getting-started-with-mvc-part3/_static/image14.png)](getting-started-with-mvc-part3/_static/image13.png)

Tutto ciò rappresenta una sorta di "M" per il modello, ma non il tipo di database. Diamo ciò che abbiamo appreso e creare un database di film.

> [!div class="step-by-step"]
> [Precedente](getting-started-with-mvc-part2.md)
> [Successivo](getting-started-with-mvc-part4.md)
