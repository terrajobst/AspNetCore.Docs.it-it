---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Creare un nuovo progetto ASP.NET MVC | Documenti Microsoft
author: microsoft
description: Passaggio 1 viene illustrato come inserire la struttura di base NerdDinner applicazione sul posto.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: d15ca67f0ddd8db6842bc5112996ae2dee433536
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="create-a-new-aspnet-mvc-project"></a>Creare un nuovo progetto ASP.NET MVC
====================
by [Microsoft](https://github.com/microsoft)

[Scarica il PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Fase 1 del processo di una liberazione [esercitazione applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che si interromperanno-through come compilare un piccolo, ma completa, un'applicazione web con ASP.NET MVC 1.
> 
> Passaggio 1 viene illustrato come inserire la struttura di base NerdDinner applicazione sul posto.
> 
> Se si utilizza ASP.NET MVC 3, è consigliabile seguire il [recupero avviato con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [negozio MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) esercitazioni.


## <a name="nerddinner-step-1-file-gtnew-project"></a>NerdDinner passaggio 1: File -&gt;nuovo progetto

L'applicazione NerdDinner inizieremo selezionando il **File -&gt;nuovo progetto** voce di menu all'interno di Visual Studio 2008 o il libero Visual Web Developer 2008 Express.

Verrà visualizzata la finestra di dialogo "Nuovo progetto". Per creare una nuova applicazione MVC ASP.NET, si sarà selezionare il nodo "Web" sul lato sinistro della finestra di dialogo e quindi scegliere il modello di progetto "Applicazione Web MVC ASP.NET" a destra:

![](create-a-new-aspnet-mvc-project/_static/image1.png)

*Importante: Assicurarsi aver scaricato e installato ASP.NET MVC, in caso contrario non verranno visualizzati nella finestra di dialogo Nuovo progetto. È possibile utilizzare V2 del [installazione guidata piattaforma Web di Microsoft](https://www.microsoft.com/web/downloads/platform.aspx) se è ancora installato ancora (ASP.NET MVC è disponibile all'interno di "piattaforma Web -&gt;Framework e runtime" sezione).*

Sarà denominato il nuovo progetto che verrà creare "NerdDinner" e quindi fare clic sul pulsante "ok" per crearlo.

Quando si fa clic su "ok" Visual Studio verrà visualizzata una finestra di dialogo aggiuntiva che richiede di creare facoltativamente un progetto di unit test per la nuova applicazione. Questo progetto di unit test consente di creare test automatizzati per verificare le funzionalità e il comportamento dell'applicazione (un elemento ci occuperemo come attività da eseguire più avanti in questa esercitazione).

![](create-a-new-aspnet-mvc-project/_static/image2.png)

L'elenco a discesa "Framework di Test" nella finestra di dialogo precedente viene popolata con tutti disponibili ASP.NET MVC unit test di modelli di progetto installato nel computer. È possibile scaricare le versioni per NUnit, MBUnit e XUnit. È inoltre supportato il framework di Unit Test di Visual Studio incorporato.

*Nota: Il Framework Unit Test di Visual Studio è disponibile solo con Visual Studio 2008 Professional e versioni successive. Se si utilizza Visual Studio 2008 Standard Edition o Visual Web Developer 2008 Express è necessario scaricare e installare le estensioni NUnit, MBUnit o XUnit per ASP.NET MVC per questa finestra di dialogo da visualizzare. Se non è installato alcun framework di test, la finestra di dialogo non verrà visualizzata.*

Verrà usata il nome "NerdDinner.Tests" predefinito per il progetto di test che verrà creata e utilizzare l'opzione di framework "Unit Test con Visual Studio". Quando si fa clic sul pulsante "ok" Visual Studio creerà una soluzione per noi con due progetti in essa contenuti, uno per l'applicazione web e uno per i test di unità:

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a>Esame della struttura di directory NerdDinner

Quando si crea una nuova applicazione MVC ASP.NET con Visual Studio, aggiunge automaticamente un numero di file e directory per il progetto:

![](create-a-new-aspnet-mvc-project/_static/image4.png)

Progetti ASP.NET MVC per impostazione predefinita sono sei directory di primo livello:

| **Directory** | **Scopo** |
| --- | --- |
| **/Controllers** | In cui inserire le classi Controller che gestiscono le richieste di URL |
| **O i modelli** | In cui inserire le classi che rappresentano e modificano i dati |
| **/Views** | In cui inserire i file di modello dell'interfaccia utente che sono responsabili dell'output del rendering |
| **/Scripts** | In cui inserire i file di libreria JavaScript e script (con estensione js) |
| **/Content** | In cui inserire CSS e i file di immagine e altro contenuto non non dinamico/JavaScript |
| **/App\_Data** | Se si archiviano file di dati che si desidera lettura/scrittura. |

ASP.NET MVC non richiede questa struttura. In realtà, gli sviluppatori che lavorano in applicazioni di grandi dimensioni in genere suddividerà l'applicazione backup tra più progetti per renderla più gestibile (ad esempio: classi di modello di dati spesso inseriti in un progetto libreria di classi separato dall'applicazione web). Struttura del progetto predefinita, tuttavia, fornisce una convenzione di directory nice predefinito che è possibile usare per mantenere pulita la problematiche relative all'applicazione.

Quando si espande la directory /Controllers verrà individuato che Visual Studio ha aggiunto due classi controller, HomeController e AccountController, per impostazione predefinita al progetto:

![](create-a-new-aspnet-mvc-project/_static/image5.png)

Quando si espande la directory /Views, ci sono disponibili tre sottodirectory: /Home, /Account e /Shared:, nonché il modello più file all'interno di essi sono stati anche aggiunti al progetto per impostazione predefinita:

![](create-a-new-aspnet-mvc-project/_static/image6.png)

Quando si espande i /Content e le directory /script, è possibile trovare un file Site.css che viene utilizzato per definire lo stile di tutto il codice HTML nel sito, nonché le librerie JavaScript che è possono abilitare ASP.NET AJAX e jQuery il supporto all'interno dell'applicazione:

![](create-a-new-aspnet-mvc-project/_static/image7.png)

Quando si espande il progetto NerdDinner.Tests ci sono disponibili due classi che contengono gli unit test per il nostro classi controller:

![](create-a-new-aspnet-mvc-project/_static/image8.png)

Questi file predefinito aggiunti da Visual Studio forniscono con una struttura di base per un'applicazione funzionante - completa con una pagina home, sulla pagina, le pagine di accesso/disconnessione/registrazione account e una pagina di errore non gestito (tutte le reti cablate-up e in esecuzione all'esterno della casella).

### <a name="running-the-nerddinner-application"></a>Esecuzione dell'applicazione NerdDinner

È possibile eseguire il progetto scegliendo il **Debug -&gt;Avvia debug** o **Debug -&gt;Avvia senza eseguire debug** voci di menu:

![](create-a-new-aspnet-mvc-project/_static/image9.png)

Questo verrà avviare il server Web ASP.NET predefinito, che viene fornito con Visual Studio ed eseguire l'applicazione:

![](create-a-new-aspnet-mvc-project/_static/image10.png)

Di seguito è la home page per il nuovo progetto (URL: "/") al momento dell'esecuzione:

![](create-a-new-aspnet-mvc-project/_static/image11.png)

Fare clic sulla scheda "Informazioni su" Visualizza una pagina (URL: "/ Home/About"):

![](create-a-new-aspnet-mvc-project/_static/image12.png)

Facendo clic sul collegamento "Accedi" in alto a destra si riferisce a una pagina di accesso (URL: "/ Account/accesso")

![](create-a-new-aspnet-mvc-project/_static/image13.png)

Se non è un account di accesso è possibile fare clic sul collegamento di registrazione (URL: "/ Account/Register") per creare uno:

![](create-a-new-aspnet-mvc-project/_static/image14.png)

Il codice per implementare la home page precedente, e disconnessione / registrare funzionalità è stato aggiunto per impostazione predefinita, quando abbiamo creato il nuovo progetto. È possibile usarla come il punto di partenza dell'applicazione.

### <a name="testing-the-nerddinner-application"></a>Test dell'applicazione NerdDinner

Se si sta usando la Professional Edition o versione successiva di Visual Studio 2008, è possibile utilizzare l'unità predefinita test supporto IDE di Visual Studio per il progetto di test:

![](create-a-new-aspnet-mvc-project/_static/image15.png)

Scegliere una delle opzioni precedenti verranno aprire il riquadro "Risultati dei Test" all'interno dell'IDE e fornire lo stato di esito positivo o negativo di 27 unit test inclusi nel nuovo progetto che coprono le funzionalità predefinite:

![](create-a-new-aspnet-mvc-project/_static/image16.png)

Più avanti in questa esercitazione si sarà ulteriori informazioni sull'esecuzione di test automatizzati e aggiungere unit test aggiuntivi che illustrano la funzionalità dell'applicazione che viene implementato.

### <a name="next-step"></a>Passo successivo

Abbiamo ora una struttura di base dell'applicazione sul posto. Verrà ora [creare un database per archiviare i dati dell'applicazione](create-a-database.md).

> [!div class="step-by-step"]
> [Precedente](introducing-the-nerddinner-tutorial.md)
> [Successivo](create-a-database.md)
