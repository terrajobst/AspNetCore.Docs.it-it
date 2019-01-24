---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: Usando i contatori delle prestazioni di SignalR in un ruolo Web di Azure | Microsoft Docs
author: guardrex
description: Come installare e usare contatori delle prestazioni di SignalR in un ruolo Web di Azure.
keywords: Contatore ASP.NET,SignalR,Performance, ruolo web di azure
ms.author: bradyg
ms.date: 10/03/2018
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 8e17e945bc144731dd149bd7ddfc9e29160eaf0b
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836483"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a>Usando i contatori delle prestazioni di SignalR in un ruolo Web di Azure

Di [Luke Latham](https://github.com/guardrex)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

I contatori delle prestazioni di SignalR vengono utilizzati per monitorare le prestazioni dell'app in un ruolo Web di Azure. I contatori vengono acquisiti da diagnostica di Microsoft Azure. Si installa i contatori delle prestazioni di SignalR in Azure con *signalr.exe*, lo stesso strumento usato per le app autonome o in locale. Poiché i ruoli di Azure sono temporanei, si configura un'app per installare e registrare i contatori delle prestazioni di SignalR all'avvio.

## <a name="prerequisites"></a>Prerequisiti

* Visual Studio 2015 o [2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
* [Microsoft Azure SDK per Visual Studio](https://azure.microsoft.com/downloads/) **Nota: Riavviare il computer dopo aver installato il SDK.**
* Sottoscrizione di Microsoft Azure: Per iscriversi a un account di Azure gratuito, vedere [valutazione gratuita di Azure](https://azure.microsoft.com/free/).

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a>Creazione di un'applicazione ruolo Web di Azure che espone i contatori delle prestazioni di SignalR

1. Aprire Visual Studio.

2. In Visual Studio selezionare **File** > **Nuovo** > **Progetto**.

3. Nel **nuovo progetto** finestra di dialogo, seleziona la **Visual c#** > **Cloud** categorie a sinistra e quindi selezionare il **servizioClouddiAzure** modello. Denominare l'app **SignalRPerfCounters** e selezionare **OK**.

   ![Nuova applicazione Cloud](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)

   > [!NOTE]
   > Se non viene visualizzato il **Cloud** categoria del modello o la **servizio Cloud di Azure** modello, è necessario installare il **sviluppo di Azure** carico di lavoro per Visual Studio 2017. Scegliere il **aprire il processo di installazione di Visual Studio** collegamento sul lato inferiore sinistro della **nuovo progetto** finestra di dialogo per aprire l'installazione di Visual Studio. Selezionare il **sviluppo di Azure** carico di lavoro, quindi scegliere **Modify** per avviare l'installazione del carico di lavoro.
   >
   > ![Carico di lavoro di sviluppo di Azure nel programma di installazione di Visual Studio](using-signalr-performance-counters-in-an-azure-web-role/_static/azure-development-workload.png)

4. Nel **nuovo servizio Cloud di Microsoft Azure** finestra di dialogo, seleziona **ruolo Web ASP.NET** e selezionare il > pulsante per aggiungere il ruolo al progetto. Scegliere **OK**.

   ![Aggiungere il ruolo Web ASP.NET](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)

5. Nel **nuova applicazione Web ASP.NET - ruolo Web1** finestra di dialogo, seleziona la **MVC** modello e quindi selezionare **OK**.

   ![Aggiungere l'API Web e MVC](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)

6. Nelle **Esplora soluzioni**, aprire il *Diagnostics. wadcfgx* file sotto **WebRole1**.

   ![Diagnostics. wadcfgx Esplora soluzioni](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)

7. Sostituire il contenuto del file con la configurazione seguente e salvare il file:

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]

8. Aprire il **Console di gestione pacchetti** dalla **Tools** > **Gestione pacchetti NuGet**. Immettere i comandi seguenti per installare l'ultima versione di SignalR e il pacchetto di utilità SignalR:

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]

9. Configurare l'app per installare i contatori delle prestazioni di SignalR nell'istanza del ruolo quando viene avviato o viene riciclato. Nella **Esplora soluzioni**, fare clic sui **WebRole1** del progetto e selezionare **Add** > **nuova cartella**. Denominare la nuova cartella *avvio*.

   ![Aggiungere la cartella di avvio](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)

10. Copia il *signalr.exe* file (aggiunta con il **Microsoft.AspNet.SignalR.Utils** pacchetto) dal \<cartella del progetto > / SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\< versione > / strumenti per la *avvio* cartella creata nel passaggio precedente.

11. Nella **Esplora soluzioni**, fare doppio clic il *avvio* cartella e selezionare **Add** > **elemento esistente**. Nella finestra di dialogo visualizzata, selezionare *signalr.exe* e selezionare **Add**.

    ![Aggiungere signalr.exe al progetto](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)

12. Fare clic sui *avvio* cartella creata. Selezionare **Aggiungi** > **Nuovo elemento**. Selezionare il **generali** nodo, seleziona **File di testo**e denominare il nuovo elemento *SignalRPerfCounterInstall.cmd*. Questo file di comando installerà i contatori delle prestazioni di SignalR in ruolo web.

    ![Creare file batch di installazione dei contatori delle prestazioni di SignalR](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)

13. Quando Visual Studio crea il *SignalRPerfCounterInstall.cmd* file, verrà automaticamente aperta nella finestra principale. Sostituire il contenuto del file con lo script seguente, quindi salvare e chiudere il file. Questo script viene eseguito *signalr.exe*, che aggiunge i contatori delle prestazioni di SignalR per l'istanza del ruolo.

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]

14. Selezionare il *signalr.exe* del file in **Esplora soluzioni**. Il file **delle proprietà**, impostare **copia in Directory di Output** a **Copia sempre**.

    ![Set di copia in Directory di Output alla copia sempre](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)

15. Ripetere il passaggio precedente per il *SignalRPerfCounterInstall.cmd* file.


16. Fare clic sui *SignalRPerfCounterInstall.cmd* del file e selezionare **aperta con**. Nella finestra di dialogo visualizzata, selezionare **Editor binario** e selezionare **OK**.

    ![Apri con Editor binario](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)

17. Nell'editor binario, selezionare qualsiasi byte iniziali del file ed eliminarli. Salvare e chiudere il file.

    ![Elimina byte iniziali](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)

18. Aprire *servicedefinition. Csdef* e aggiungere un'attività di avvio che esegue il *SignalrPerfCounterInstall.cmd* all'avvio del servizio di file:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]

19. Apri `Views/Shared/_Layout.cshtml` e rimuovere lo script bundle jQuery dalla fine del file.

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]

20. Aggiungere un client JavaScript che chiama continuamente il `increment` metodo nel server. Apri `Views/Home/Index.cshtml` e sostituire il contenuto con il codice seguente:

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]

21. Creare una nuova cartella nella **WebRole1** progetto denominato *hub*. Fare doppio clic sui *hub* cartella **Esplora soluzioni** e selezionare **Add** > **nuovo elemento**. Nel **Aggiungi nuovo elemento** finestra di dialogo, seleziona la **Web** > **SignalR** categoria e quindi selezionare il **classe tramite SignalR Hub (v2)** modello di elemento. Denominare il nuovo hub *MyHub.cs* e selezionare **Add**.

    ![Aggiunta classe Hub SignalR nella cartella hub nella finestra di dialogo Aggiungi nuovo elemento](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. *MyHub.cs* verrà automaticamente aperto nella finestra principale. Sostituire il contenuto con il codice seguente, quindi salvare e chiudere il file:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]

23. *[Crank.exe](signalr-connection-density-testing-with-crank.md)*  è una strumento fornito con la codebase SignalR di test della densità di connessione. Poiché il perno richiede una connessione permanente, si aggiungerne uno al sito per l'uso durante il test. Aggiungere una nuova cartella per il **WebRole1** progetto chiamato *PersistentConnections*. Fare doppio clic su questa cartella e selezionare **Add** > **classe**. Denominare il nuovo file di classe *MyPersistentConnections.cs* e selezionare **Add**.

24. Visual Studio aprirà la *MyPersistentConnections.cs* file nella finestra principale. Sostituire il contenuto con il codice seguente, quindi salvare e chiudere il file:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]

25. Uso di `Startup` (classe), gli oggetti di SignalR avviano all'avvio di OWIN. Aprire o creare *Startup.cs* e sostituire il contenuto con il codice seguente:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]

    Nel codice precedente, il `OwinStartup` attributo contrassegna questa classe per iniziare a OWIN. Il `Configuration` metodo inizia a SignalR.

26. Test dell'applicazione nell'emulatore di Microsoft Azure premendo **F5**.

    > [!NOTE]
    > Se si verifica una **FileLoadException** alla **MapSignalR**, modificare i reindirizzamenti di associazione nelle *Web. config* al seguente:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]

27. Attendere circa un minuto. Aprire la finestra degli strumenti di Cloud Explorer in Visual Studio (**View** > **Cloud Explorer**) ed espandere il percorso `(Local)/Storage Accounts/(Development)/Tables`. Fare doppio clic su **WADPerformanceCountersTable**. Verranno visualizzati i contatori SignalR nei dati della tabella. Se non viene visualizzata la tabella, si potrebbe essere necessario immettere nuovamente le credenziali di archiviazione di Azure. Potrebbe essere necessario selezionare la **Refresh** pulsante per visualizzare la tabella in **Cloud Explorer** oppure selezionare il **Aggiorna** pulsante nella finestra Apri tabella per visualizzare i dati nella tabella.

    ![Selezionare la tabella di contatori delle prestazioni di WAD in Visual Studio Cloud Explorer](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Che mostra i contatori raccolti nella tabella di contatori delle prestazioni di WAD](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)

28. Per testare l'applicazione nel cloud, aggiornare il **ServiceConfiguration** file e impostare il `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` su una stringa di connessione account di archiviazione di Azure valida.

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. Distribuire l'applicazione alla sottoscrizione di Azure. Per informazioni dettagliate su come distribuire un'applicazione in Azure, vedere [come creare e distribuire un servizio Cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).

30. Attendere alcuni minuti. Nelle **Cloud Explorer**, individuare l'account di archiviazione configurata in precedenza e trovare il `WADPerformanceCountersTable` tabella in esso. Verranno visualizzati i contatori SignalR nei dati della tabella. Se non viene visualizzata la tabella, si potrebbe essere necessario immettere nuovamente le credenziali di archiviazione di Azure. Potrebbe essere necessario selezionare la **Refresh** pulsante per visualizzare la tabella in **Cloud Explorer** oppure selezionare il **Aggiorna** pulsante nella finestra Apri tabella per visualizzare i dati nella tabella.

Ringraziamenti speciali [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) per visualizzarne il contenuto usato in questa esercitazione.
