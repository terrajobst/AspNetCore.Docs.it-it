---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: Utilizzo di contatori delle prestazioni di SignalR in un ruolo Web Azure | Documenti Microsoft
author: guardrex
description: Come installare e utilizzare i contatori delle prestazioni di SignalR in un ruolo Web Azure.
keywords: Contatore ASP.NET,SignalR,Performance, il ruolo web di azure
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2017
ms.topic: article
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 0d2717eb318d282e21e9aa8622a205f556e3a4ee
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a>Utilizzo di contatori delle prestazioni di SignalR in un ruolo Web Azure

Di [Luke Latham](https://github.com/guardrex)

I contatori delle prestazioni di SignalR vengono utilizzati per monitorare le prestazioni dell'app in un ruolo Web Azure. I contatori vengono acquisiti da diagnostica di Microsoft Azure. Installare i contatori delle prestazioni di SignalR in Azure con *signalr.exe*, lo stesso strumento utilizzato per applicazioni autonome o in locale. Poiché i ruoli di Azure sono temporanei, configurare un'applicazione per installare e registrare i contatori delle prestazioni di SignalR all'avvio.

## <a name="prerequisites"></a>Prerequisiti

* [Visual Studio 2015](https://www.visualstudio.com/vs/visual-studio-express/)
* [Microsoft Azure SDK per Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **Nota: riavviare il computer dopo l'installazione di SDK.**
* Sottoscrizione di Microsoft Azure: per iscriversi a un account di prova Azure, vedere [Azure versione di valutazione gratuita](https://azure.microsoft.com/free/).

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a>Creazione di un'applicazione di ruolo Web Azure che espone i contatori delle prestazioni di SignalR

1. Aprire Visual Studio 2015.

2. In Visual Studio 2015, selezionare **File &gt; New &gt; progetto**.

3. Nel **modelli** riquadro del **nuovo progetto** finestra di sotto di **Visual c#** nodo, seleziona il **Cloud** nodo e selezionare il **Servizio Cloud di azure** modello. Denominare l'app **SignalRPerfCounters** e selezionare **OK**.

   ![Nuova applicazione Cloud](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)
    
4. Nel **nuovo servizio Cloud di Microsoft Azure** finestra di dialogo Seleziona **ruolo Web ASP.NET** e selezionare il  **&gt;**  pulsante per aggiungere il ruolo al progetto. Scegliere **OK**.

   ![Aggiungere il ruolo Web ASP.NET](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)
    
5. Nel **nuova applicazione Web ASP.NET - WebRole1** finestra di dialogo Seleziona il **MVC** modello e quindi selezionare **OK**.

   ![Aggiungere le API di MVC e Web](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)
    
6. In **Esplora**, aprire il *wadcfgx* file **WebRole1**.

   ![Wadcfgx di Esplora soluzioni](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)
    
7. Sostituire il contenuto del file con la configurazione seguente e salvare il file:

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]
    
8. Aprire il **Console di gestione pacchetti** da **strumenti &gt; Gestione pacchetti NuGet**. Immettere i comandi seguenti per installare la versione più recente di SignalR e il pacchetto di utilità SignalR:

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]
    
9. Configura l'applicazione per installare i contatori delle prestazioni di SignalR nell'istanza del ruolo quando avviato o viene riciclato. In **Esplora**, fare clic su di **WebRole1** del progetto e selezionare **Aggiungi &gt; nuova cartella**. Denominare la nuova cartella *avvio*.

   ![Aggiungere la cartella di avvio](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)
    
10. Copia il *signalr.exe* file (aggiunta con il **Microsoft.AspNet.SignalR.Utils** pacchetto) da  **&lt;cartella progetto&gt;\SignalRPerfCounters\packages\ Microsoft.AspNet.SignalR.Utils. &lt;versione&gt;\tools** per il *avvio* cartella creata nel passaggio precedente.

11. In **Esplora**, fare doppio clic su di *avvio* cartella e selezionare **Aggiungi &gt; elemento esistente**. Nella finestra di dialogo visualizzata selezionare *signalr.exe* e selezionare **Aggiungi**.

    ![Aggiungere signalr.exe al progetto](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)
    
12. Fare clic su di *avvio* cartella creata. Selezionare **aggiungere &gt; nuovo elemento**. Selezionare il **generale** nodo, seleziona **File di testo**e assegnare al nuovo elemento *SignalRPerfCounterInstall.cmd*. Questo file di comando installerà i contatori delle prestazioni di SignalR al ruolo web.

    ![Creare file batch di installazione contatori delle prestazioni di SignalR](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)
     
13. Quando Visual Studio crea il *SignalRPerfCounterInstall.cmd* file, verrà aperto automaticamente nella finestra principale. Sostituire il contenuto del file con lo script seguente, quindi salvare e chiudere il file. Questo script esegue *signalr.exe*, che aggiunge i contatori delle prestazioni di SignalR per l'istanza del ruolo.

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]
    
14. Selezionare il *signalr.exe* file **Esplora**. Il file **proprietà**, impostare **copia nella Directory di Output di** a **Copia sempre**.

    ![Impostare Copia nella Directory di Output per Copia sempre](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)
    
15. Ripetere il passaggio precedente per il *SignalRPerfCounterInstall.cmd* file.

    
16. Fare clic su di *SignalRPerfCounterInstall.cmd* file e selezionare **Apri con**. Nella finestra di dialogo visualizzata selezionare **Editor binario** e selezionare **OK**.

    ![Apri con Editor binario](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)
    
17. Nell'editor binario, selezionare i byte iniziali del file ed eliminarli. Salvare e chiudere il file.

    ![Elimina i byte iniziali](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)
    
18. Aprire *Servicedefinition* e aggiungere un'attività di avvio che esegue il *SignalrPerfCounterInstall.cmd* all'avvio del servizio di file:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]
    
19. Aprire `Views/Shared/_Layout.cshtml` e rimuovere lo script bundle jQuery dalla fine del file.

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]
    
20. Aggiungere un client JavaScript che chiama continuamente il `increment` metodo sul server. Aprire `Views/Home/Index.cshtml` e sostituire il contenuto con il codice seguente:

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]
    
21. Creare una nuova cartella nel **WebRole1** progetto denominato *hub*. Fare doppio clic sul *hub* cartella **Esplora**selezionare **Web &gt; SignalR**e selezionare **classe Hub SignalR (v2)**. Nome del nuovo hub *MyHub.cs* e selezionare **Aggiungi**.

    ![Classe Hub SignalR aggiunta alla cartella hub nella finestra di dialogo Aggiungi nuovo elemento](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. *MyHub.cs* verrà aperto automaticamente nella finestra principale. Sostituire il contenuto con il codice seguente, quindi salvare e chiudere il file:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]
    
23. *[Crank.exe](signalr-connection-density-testing-with-crank.md)*  densità connessione test dello strumento fornito con la codebase SignalR. Poiché il perno richiede una connessione permanente, si aggiungerne uno al sito per l'utilizzo durante il test. Aggiungere una nuova cartella per il **WebRole1** progetto denominato *PersistentConnections*. Questa cartella e scegliere **Aggiungi &gt; classe**. Denominare il nuovo file di classe *MyPersistentConnections.cs* e selezionare **Aggiungi**.

24. Visual Studio verrà aperto il *MyPersistentConnections.cs* file nella finestra principale. Sostituire il contenuto con il codice seguente, quindi salvare e chiudere il file:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]
    
25. Utilizzo di `Startup` (classe), gli oggetti SignalR avviano all'avvio di OWIN. Aprire o creare *Startup.cs* e sostituire il contenuto con il codice seguente:

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]
    
    Nel codice precedente, il `OwinStartup` questa classe per l'avvio di OWIN contrassegnato dall'attributo. Il `Configuration` metodo avvia SignalR.
    
26. Testare l'applicazione nell'emulatore di Microsoft Azure premendo **F5**.

    > [!NOTE]
    > Se si verifica un **FileLoadException** in **MapSignalR**, modificare i reindirizzamenti di associazione in *Web. config* al seguente:

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]
    
27. Attendere circa un minuto. Aprire la finestra degli strumenti Cloud Explorer in Visual Studio (**vista &gt; Cloud Explorer**) ed espandere il percorso `(Local)\Storage Accounts\(Development)\Tables`. Fare doppio clic su **WADPerformanceCountersTable**. Dovrebbe essere contatori SignalR nei dati della tabella. Se non viene visualizzato nella tabella, devi immettere nuovamente le credenziali di archiviazione di Azure. È necessario selezionare il **aggiornare** pulsante per visualizzare la tabella in **Cloud Explorer** o selezionare il **aggiornamento** pulsante nella finestra Apri tabella per visualizzare i dati nella tabella.

    ![Selezionando la tabella di contatori delle prestazioni di WAD in Visual Studio Cloud Explorer](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Visualizzare i contatori raccolti nella tabella di contatori delle prestazioni di WAD](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)
    
28. Per testare l'applicazione nel cloud, aggiornare il **ServiceConfiguration** file e impostare il `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` su una stringa di connessione di account di archiviazione di Azure valida.

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. Distribuire l'applicazione alla sottoscrizione di Azure. Per informazioni dettagliate su come distribuire un'applicazione in Azure, vedere [come creare e distribuire un servizio Cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).

30. Attendere qualche minuto. In **Cloud Explorer**, individuare l'account di archiviazione è configurato in precedenza e trovare il `WADPerformanceCountersTable` tabella in essa contenuti. Dovrebbe essere contatori SignalR nei dati della tabella. Se non viene visualizzato nella tabella, devi immettere nuovamente le credenziali di archiviazione di Azure. È necessario selezionare il **aggiornare** pulsante per visualizzare la tabella in **Cloud Explorer** o selezionare il **aggiornamento** pulsante nella finestra Apri tabella per visualizzare i dati nella tabella.

Ringraziamenti speciali [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) per il contenuto originale utilizzato in questa esercitazione.
