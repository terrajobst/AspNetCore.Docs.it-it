---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Creare un nuovo progetto ASP.NET MVC | Microsoft Docs
author: microsoft
description: Passaggio 1 viene illustrato come implementare la struttura dell'applicazione di NerdDinner base.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 9f5e0b3f82d113fc72ab4002ec8d06ad8444dceb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374276"
---
<a name="create-a-new-aspnet-mvc-project"></a>Creare un nuovo progetto ASP.NET MVC
====================
da [Microsoft](https://github.com/microsoft)

[Scaricare PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Si tratta di passaggio 1 di una liberazione [esercitazione sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che si interromperanno-dettaglio come compilare una piccola, ma completa, applicazione web con ASP.NET MVC 1.
> 
> Passaggio 1 viene illustrato come implementare la struttura dell'applicazione di NerdDinner base.
> 
> Se si usa ASP.NET MVC 3, è consigliabile seguire le [Guida introduttiva con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oppure [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.


## <a name="nerddinner-step-1-file-gtnew-project"></a>NerdDinner passaggio 1: File -&gt;nuovo progetto

Inizieremo la nostra applicazione NerdDinner selezionando il **File -&gt;nuovo progetto** voce di menu all'interno di Visual Studio 2008 o il gratuito Visual Web Developer 2008 Express.

Verrà visualizzata la finestra di dialogo "Nuovo progetto". Per creare una nuova applicazione MVC ASP.NET, si sarà selezionare il nodo "Web" sul lato sinistro della finestra di dialogo e quindi scegliere il modello di progetto "Applicazione Web ASP.NET MVC" a destra:

![](create-a-new-aspnet-mvc-project/_static/image1.png)

*Importante: Verificare che è stata scaricata e installata ASP.NET MVC - in caso contrario non verrà visualizzato nella finestra di dialogo Nuovo progetto. È possibile usare V2 del [installazione guidata piattaforma Web Microsoft](https://www.microsoft.com/web/downloads/platform.aspx) se è ancora installato ancora (ASP.NET MVC è disponibile all'interno di "piattaforma Web -&gt;Framework e runtime" sezione).*

Sarà denominato nuovo progetto che si intende creare "NerdDinner" e quindi fare clic sul pulsante "ok" per crearlo.

Quando si fa clic su "ok" Visual Studio verrà visualizzata una finestra di dialogo aggiuntiva che consente, facoltativamente, creare un progetto di unit test per la nuova applicazione. Questo progetto di unit test ci permette di creare test automatizzati che consentono di verificare le funzionalità e il comportamento dell'applicazione (qualcosa si affronterà il modo in cui le cose da fare più avanti in questa esercitazione).

![](create-a-new-aspnet-mvc-project/_static/image2.png)

L'elenco a discesa "Framework di Test" nella finestra di dialogo precedente viene popolato con tutti i disponibili ASP.NET MVC unit test di modelli di progetto installati nel computer. Le versioni possono essere scaricate per MBUnit, NUnit e XUnit. È supportato anche il framework di Unit Test di Visual Studio incorporato.

*Nota: Il Framework Unit Test di Visual Studio è disponibile solo con Visual Studio 2008 Professional e versioni successive. Se si usa Visual Studio 2008 Standard Edition o Visual Web Developer 2008 Express è necessario scaricare e installare le estensioni di NUnit, MBUnit o XUnit per ASP.NET MVC di questa finestra di dialogo da visualizzare. Se non ci sono qualsiasi framework di test installato, non verrà visualizzata la finestra di dialogo.*

È possibile utilizzare quello predefinito "NerdDinner.Tests" per il progetto di test che viene creata e usare l'opzione di framework "Unit Test con Visual Studio". Quando si fa clic sul pulsante "ok" Visual Studio verrà creata una soluzione per noi con due progetti in essa - uno per la nostra applicazione web e uno per i test di unità:

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a>Esame della struttura di directory di NerdDinner

Quando si crea una nuova applicazione ASP.NET MVC con Visual Studio, lo aggiunge automaticamente un numero di file e directory al progetto:

![](create-a-new-aspnet-mvc-project/_static/image4.png)

I progetti ASP.NET MVC per impostazione predefinita sono sei directory di primo livello:

| **Directory** | **Scopo** |
| --- | --- |
| **/ Controller** | Si inseriranno le classi Controller che gestiscono le richieste di URL |
| **O i modelli** | Si inseriranno le classi che rappresentano e modificano i dati |
| **/ Viste** | In cui vengono salvati i file di modello di interfaccia utente che sono responsabili di output del rendering |
| **/ Script** | Si inseriranno i file di libreria JavaScript e script (con estensione js) |
| **/Content** | Si inseriranno CSS e file di immagine e altri contenuti non non dinamica/JavaScript |
| **/App\_Data** | In cui si archiviano file di dati che si desidera lettura/scrittura. |

ASP.NET MVC non richiede questa struttura. In effetti, gli sviluppatori che lavorano su applicazioni di grandi dimensioni in genere suddividerà l'applicazione backup tra più progetti per renderla più gestibile (ad esempio: classi di modello di dati spesso passare in un progetto di libreria di classi separata dall'applicazione web). La struttura del progetto predefinita, tuttavia, fornisce una convenzione di directory predefinito interessante che è possibile usare per mantenere pulito i problemi dell'applicazione.

Quando si espande la directory /Controllers verrà individuato che Visual Studio ha aggiunto due classi controller, HomeController e AccountController, per impostazione predefinita al progetto:

![](create-a-new-aspnet-mvc-project/_static/image5.png)

Quando si espande la directory di modelli, è possibile trovare tre sottodirectory: avremo, /Account e /Shared – nonché modelli diversi all'interno di essi sono stati anche aggiunti file al progetto per impostazione predefinita:

![](create-a-new-aspnet-mvc-project/_static/image6.png)

Quando si espande il /Content e le directory /script, è possibile trovare un file Site CSS utilizzato per applicare uno stile a tutto il codice HTML nel sito, nonché librerie JavaScript che è possono abilitare ASP.NET AJAX e jQuery supportano all'interno dell'applicazione:

![](create-a-new-aspnet-mvc-project/_static/image7.png)

Quando si espande il progetto NerdDinner.Tests verrà individuato due classi che contengono gli unit test per le classi controller:

![](create-a-new-aspnet-mvc-project/_static/image8.png)

Questi file predefinito aggiunti da Visual Studio forniscono una struttura di base per un'applicazione funzionante - completa con una pagina home, sulla pagina, le pagine di accesso/disconnessione/registrazione account e una pagina di errore non gestito (tutto dopo aver e lavoro impostazione predefinita).

### <a name="running-the-nerddinner-application"></a>Esecuzione dell'applicazione di NerdDinner

È possibile eseguire il progetto scegliendo tra le **Debug -&gt;Avvia debug** oppure **Debug -&gt;Avvia senza eseguire debug** voci di menu:

![](create-a-new-aspnet-mvc-project/_static/image9.png)

Questo verrà avviare il server Web ASP.NET predefinito-fornito con Visual Studio ed eseguire l'applicazione:

![](create-a-new-aspnet-mvc-project/_static/image10.png)

Di seguito è la home page per il nuovo progetto (URL: "/") al momento dell'esecuzione:

![](create-a-new-aspnet-mvc-project/_static/image11.png)

Facendo clic sulla scheda "About" consente di visualizzare sulla pagina (URL: "/ Home/About"):

![](create-a-new-aspnet-mvc-project/_static/image12.png)

Facendo clic sul collegamento "Accedi" in alto a destra, viene visualizzata una pagina di accesso (URL: "/ Account/LogOn")

![](create-a-new-aspnet-mvc-project/_static/image13.png)

Se si ha un account di accesso è possibile fare clic sul collegamento register (URL: "/ Account/Register") per crearne uno:

![](create-a-new-aspnet-mvc-project/_static/image14.png)

Il codice per implementare la precedente home page, e la disconnessione o registrare la funzionalità è stata aggiunta per impostazione predefinita, quando abbiamo creato il nuovo progetto. Si userà lo come punto di partenza dell'applicazione.

### <a name="testing-the-nerddinner-application"></a>Test dell'applicazione di NerdDinner

Se si sta usando la Professional Edition o versione successiva di Visual Studio 2008, è possibile usare l'unità predefinito test supporto IDE di Visual Studio per il progetto di test:

![](create-a-new-aspnet-mvc-project/_static/image15.png)

Scegliere una delle opzioni precedenti verranno aprire il riquadro "Risultati dei Test" all'interno dell'IDE e fornire lo stato di esito positivo o negativo nel 27 unit test inclusi nel nostro nuovo progetto che coprono le funzionalità predefinite:

![](create-a-new-aspnet-mvc-project/_static/image16.png)

Più avanti in questa esercitazione verranno fornite altre informazioni di test automatizzati e aggiungere ulteriori unit test che coprono le funzionalità dell'applicazione, che abbiamo implementato.

### <a name="next-step"></a>Passo successivo

A questo punto, abbiamo una struttura di base dell'applicazione posto. È possibile ora [creare un database per archiviare i dati applicazione](create-a-database.md).

> [!div class="step-by-step"]
> [Precedente](introducing-the-nerddinner-tutorial.md)
> [Successivo](create-a-database.md)
