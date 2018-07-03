---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: Usando i contatori delle prestazioni di SignalR in un ruolo Web di Azure | Microsoft Docs
author: guardrex
description: Come installare e usare contatori delle prestazioni di SignalR in un ruolo Web di Azure.
keywords: Contatore ASP.NET,SignalR,Performance, ruolo web di azure
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2017
ms.topic: article
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: ffc8033ca58a3ff559eacdd1cd14e77bfc692a31
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378044"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a><span data-ttu-id="9d384-104">Usando i contatori delle prestazioni di SignalR in un ruolo Web di Azure</span><span class="sxs-lookup"><span data-stu-id="9d384-104">Using SignalR performance counters in an Azure Web Role</span></span>

<span data-ttu-id="9d384-105">Di [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9d384-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="9d384-106">I contatori delle prestazioni di SignalR vengono utilizzati per monitorare le prestazioni dell'app in un ruolo Web di Azure.</span><span class="sxs-lookup"><span data-stu-id="9d384-106">SignalR performance counters are used to monitor your app's performance in an Azure Web Role.</span></span> <span data-ttu-id="9d384-107">I contatori vengono acquisiti da diagnostica di Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="9d384-107">The counters are captured by Microsoft Azure Diagnostics.</span></span> <span data-ttu-id="9d384-108">Si installa i contatori delle prestazioni di SignalR in Azure con *signalr.exe*, lo stesso strumento usato per le app autonome o in locale.</span><span class="sxs-lookup"><span data-stu-id="9d384-108">You install SignalR performance counters on Azure with *signalr.exe*, the same tool used for standalone or on-premises apps.</span></span> <span data-ttu-id="9d384-109">Poiché i ruoli di Azure sono temporanei, si configura un'app per installare e registrare i contatori delle prestazioni di SignalR all'avvio.</span><span class="sxs-lookup"><span data-stu-id="9d384-109">Since Azure roles are transient, you configure an app to install and register SignalR performance counters upon startup.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9d384-110">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="9d384-110">Prerequisites</span></span>

* [<span data-ttu-id="9d384-111">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="9d384-111">Visual Studio 2015</span></span>](https://www.visualstudio.com/vs/visual-studio-express/)
* <span data-ttu-id="9d384-112">[Microsoft Azure SDK per Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **Nota: riavviare il computer dopo aver installato il SDK.**</span><span class="sxs-lookup"><span data-stu-id="9d384-112">[Microsoft Azure SDK for Visual Studio 2015 (VS2015)](https://azure.microsoft.com/downloads/) **Note: Restart your machine after installing the SDK.**</span></span>
* <span data-ttu-id="9d384-113">Sottoscrizione di Microsoft Azure: per l'iscrizione per un account di Azure gratuito, vedere [valutazione gratuita di Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="9d384-113">Microsoft Azure subscription: To sign up for a free Azure trial account, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a><span data-ttu-id="9d384-114">Creazione di un'applicazione ruolo Web di Azure che espone i contatori delle prestazioni di SignalR</span><span class="sxs-lookup"><span data-stu-id="9d384-114">Creating an Azure Web Role application that exposes SignalR performance counters</span></span>

1. <span data-ttu-id="9d384-115">Aprire Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="9d384-115">Open Visual Studio 2015.</span></span>

2. <span data-ttu-id="9d384-116">In Visual Studio 2015, selezionare **File** > **New** > **progetto**.</span><span class="sxs-lookup"><span data-stu-id="9d384-116">In Visual Studio 2015, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="9d384-117">Nel **modelli** riquadro della finestra il **nuovo progetto** finestra sotto il **Visual c#** nodo, seleziona il **Cloud** nodo e selezionare il **Servizio Cloud di azure** modello.</span><span class="sxs-lookup"><span data-stu-id="9d384-117">In the **Templates** pane of the **New Project** window under the **Visual C#** node, select the **Cloud** node and select the **Azure Cloud Service** template.</span></span> <span data-ttu-id="9d384-118">Denominare l'app **SignalRPerfCounters** e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="9d384-118">Name the app **SignalRPerfCounters** and select **OK**.</span></span>

   ![Nuova applicazione Cloud](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)
    
4. <span data-ttu-id="9d384-120">Nel **nuovo servizio Cloud di Microsoft Azure** finestra di dialogo, seleziona **ruolo Web ASP.NET** e selezionare il > pulsante per aggiungere il ruolo al progetto.</span><span class="sxs-lookup"><span data-stu-id="9d384-120">In the **New Microsoft Azure Cloud Service** dialog, select **ASP.NET Web Role** and select the > button to add the role to the project.</span></span> <span data-ttu-id="9d384-121">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="9d384-121">Select **OK**.</span></span>

   ![Aggiungere il ruolo Web ASP.NET](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)
    
5. <span data-ttu-id="9d384-123">Nel **nuova applicazione Web ASP.NET - ruolo Web1** finestra di dialogo, seleziona la **MVC** modello e quindi selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="9d384-123">In the **New ASP.NET Web Application - WebRole1** dialog, select the **MVC** template, and then select **OK**.</span></span>

   ![Aggiungere l'API Web e MVC](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)
    
6. <span data-ttu-id="9d384-125">Nelle **Esplora soluzioni**, aprire il *Diagnostics. wadcfgx* file sotto **WebRole1**.</span><span class="sxs-lookup"><span data-stu-id="9d384-125">In **Solution Explorer**, open the *diagnostics.wadcfgx* file under **WebRole1**.</span></span>

   ![Diagnostics. wadcfgx Esplora soluzioni](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)
    
7. <span data-ttu-id="9d384-127">Sostituire il contenuto del file con la configurazione seguente e salvare il file:</span><span class="sxs-lookup"><span data-stu-id="9d384-127">Replace the contents of the file with the following configuration and save the file:</span></span>

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]
    
8. <span data-ttu-id="9d384-128">Aprire il **Console di gestione pacchetti** dalla **Tools** > **Gestione pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="9d384-128">Open the **Package Manager Console** from **Tools** > **NuGet Package Manager**.</span></span> <span data-ttu-id="9d384-129">Immettere i comandi seguenti per installare l'ultima versione di SignalR e il pacchetto di utilità SignalR:</span><span class="sxs-lookup"><span data-stu-id="9d384-129">Enter the following commands to install the latest version of SignalR and the SignalR utilities package:</span></span>

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]
    
9. <span data-ttu-id="9d384-130">Configurare l'app per installare i contatori delle prestazioni di SignalR nell'istanza del ruolo quando viene avviato o viene riciclato.</span><span class="sxs-lookup"><span data-stu-id="9d384-130">Configure the app to install the SignalR performance counters into the role instance when it starts up or recycles.</span></span> <span data-ttu-id="9d384-131">Nella **Esplora soluzioni**, fare clic sui **WebRole1** del progetto e selezionare **Add** > **nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="9d384-131">In **Solution Explorer**, right-click on the **WebRole1** project and select **Add** > **New Folder**.</span></span> <span data-ttu-id="9d384-132">Denominare la nuova cartella *avvio*.</span><span class="sxs-lookup"><span data-stu-id="9d384-132">Name the new folder *Startup*.</span></span>

   ![Aggiungere la cartella di avvio](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)
    
10. <span data-ttu-id="9d384-134">Copia il *signalr.exe* file (aggiunta con il **Microsoft.AspNet.SignalR.Utils** pacchetto) dal \<cartella del progetto > / SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\< versione > / strumenti per la *avvio* cartella creata nel passaggio precedente.</span><span class="sxs-lookup"><span data-stu-id="9d384-134">Copy the *signalr.exe* file (added with the **Microsoft.AspNet.SignalR.Utils** package) from \<project folder>/SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\<version>/tools to the *Startup* folder you created in the previous step.</span></span>

11. <span data-ttu-id="9d384-135">Nella **Esplora soluzioni**, fare doppio clic il *avvio* cartella e selezionare **Add** > **elemento esistente**.</span><span class="sxs-lookup"><span data-stu-id="9d384-135">In **Solution Explorer**, right-click the *Startup* folder and select **Add** > **Existing Item**.</span></span> <span data-ttu-id="9d384-136">Nella finestra di dialogo visualizzata, selezionare *signalr.exe* e selezionare **Add**.</span><span class="sxs-lookup"><span data-stu-id="9d384-136">In the dialog that appears, select *signalr.exe* and select **Add**.</span></span>

    ![Aggiungere signalr.exe al progetto](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)
    
12. <span data-ttu-id="9d384-138">Fare clic sui *avvio* cartella creata.</span><span class="sxs-lookup"><span data-stu-id="9d384-138">Right-click on the *Startup* folder you created.</span></span> <span data-ttu-id="9d384-139">Selezionare **Aggiungi** > **Nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="9d384-139">Select **Add** > **New Item**.</span></span> <span data-ttu-id="9d384-140">Selezionare il **generali** nodo, seleziona **File di testo**e denominare il nuovo elemento *SignalRPerfCounterInstall.cmd*.</span><span class="sxs-lookup"><span data-stu-id="9d384-140">Select the **General** node, select **Text File**, and name the new item *SignalRPerfCounterInstall.cmd*.</span></span> <span data-ttu-id="9d384-141">Questo file di comando installerà i contatori delle prestazioni di SignalR in ruolo web.</span><span class="sxs-lookup"><span data-stu-id="9d384-141">This command file will install the SignalR performance counters into the web role.</span></span>

    ![Creare file batch di installazione dei contatori delle prestazioni di SignalR](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)
     
13. <span data-ttu-id="9d384-143">Quando Visual Studio crea il *SignalRPerfCounterInstall.cmd* file, verrà automaticamente aperta nella finestra principale.</span><span class="sxs-lookup"><span data-stu-id="9d384-143">When Visual Studio creates the *SignalRPerfCounterInstall.cmd* file, it will automatically open in the main window.</span></span> <span data-ttu-id="9d384-144">Sostituire il contenuto del file con lo script seguente, quindi salvare e chiudere il file.</span><span class="sxs-lookup"><span data-stu-id="9d384-144">Replace the contents of the file with the following script, then save and close the file.</span></span> <span data-ttu-id="9d384-145">Questo script viene eseguito *signalr.exe*, che aggiunge i contatori delle prestazioni di SignalR per l'istanza del ruolo.</span><span class="sxs-lookup"><span data-stu-id="9d384-145">This script executes *signalr.exe*, which adds the SignalR performance counters to the role instance.</span></span>

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]
    
14. <span data-ttu-id="9d384-146">Selezionare il *signalr.exe* del file in **Esplora soluzioni**.</span><span class="sxs-lookup"><span data-stu-id="9d384-146">Select the *signalr.exe* file in **Solution Explorer**.</span></span> <span data-ttu-id="9d384-147">Il file **delle proprietà**, impostare **copia in Directory di Output** a **Copia sempre**.</span><span class="sxs-lookup"><span data-stu-id="9d384-147">In the file's **Properties**, set **Copy to Output Directory** to **Copy Always**.</span></span>

    ![Set di copia in Directory di Output alla copia sempre](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)
    
15. <span data-ttu-id="9d384-149">Ripetere il passaggio precedente per il *SignalRPerfCounterInstall.cmd* file.</span><span class="sxs-lookup"><span data-stu-id="9d384-149">Repeat the previous step for the *SignalRPerfCounterInstall.cmd* file.</span></span>

    
16. <span data-ttu-id="9d384-150">Fare clic sui *SignalRPerfCounterInstall.cmd* del file e selezionare **aperta con**.</span><span class="sxs-lookup"><span data-stu-id="9d384-150">Right-click on the *SignalRPerfCounterInstall.cmd* file and select **Open With**.</span></span> <span data-ttu-id="9d384-151">Nella finestra di dialogo visualizzata, selezionare **Editor binario** e selezionare **OK**.</span><span class="sxs-lookup"><span data-stu-id="9d384-151">In the dialog that appears, select **Binary Editor** and select **OK**.</span></span>

    ![Apri con Editor binario](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)
    
17. <span data-ttu-id="9d384-153">Nell'editor binario, selezionare qualsiasi byte iniziali del file ed eliminarli.</span><span class="sxs-lookup"><span data-stu-id="9d384-153">In the binary editor, select any leading bytes in the file and delete them.</span></span> <span data-ttu-id="9d384-154">Salvare e chiudere il file.</span><span class="sxs-lookup"><span data-stu-id="9d384-154">Save and close the file.</span></span>

    ![Elimina byte iniziali](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)
    
18. <span data-ttu-id="9d384-156">Aprire *servicedefinition. Csdef* e aggiungere un'attività di avvio che esegue il *SignalrPerfCounterInstall.cmd* all'avvio del servizio di file:</span><span class="sxs-lookup"><span data-stu-id="9d384-156">Open *ServiceDefinition.csdef* and add a startup task that executes the *SignalrPerfCounterInstall.cmd* file when the service starts up:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]
    
19. <span data-ttu-id="9d384-157">Apri `Views/Shared/_Layout.cshtml` e rimuovere lo script bundle jQuery dalla fine del file.</span><span class="sxs-lookup"><span data-stu-id="9d384-157">Open `Views/Shared/_Layout.cshtml` and remove the jQuery bundle script from the end of the file.</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]
    
20. <span data-ttu-id="9d384-158">Aggiungere un client JavaScript che chiama continuamente il `increment` metodo nel server.</span><span class="sxs-lookup"><span data-stu-id="9d384-158">Add a JavaScript client that continuously calls the `increment` method on the server.</span></span> <span data-ttu-id="9d384-159">Apri `Views/Home/Index.cshtml` e sostituire il contenuto con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="9d384-159">Open `Views/Home/Index.cshtml` and replace the contents with the following code:</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]
    
21. <span data-ttu-id="9d384-160">Creare una nuova cartella nella **WebRole1** progetto denominato *hub*.</span><span class="sxs-lookup"><span data-stu-id="9d384-160">Create a new folder in the **WebRole1** project named *Hubs*.</span></span> <span data-ttu-id="9d384-161">Fare doppio clic il *hub* cartella **Esplora soluzioni**, selezionare **Web** > **SignalR**e selezionare  **Classe SignalR Hub (v2)**.</span><span class="sxs-lookup"><span data-stu-id="9d384-161">Right-click the *Hubs* folder in **Solution Explorer**, select **Web** > **SignalR**, and select **SignalR Hub Class (v2)**.</span></span> <span data-ttu-id="9d384-162">Denominare il nuovo hub *MyHub.cs* e selezionare **Add**.</span><span class="sxs-lookup"><span data-stu-id="9d384-162">Name the new hub *MyHub.cs* and select **Add**.</span></span>

    ![Aggiunta classe Hub SignalR nella cartella hub nella finestra di dialogo Aggiungi nuovo elemento](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. <span data-ttu-id="9d384-164">*MyHub.cs* verrà automaticamente aperto nella finestra principale.</span><span class="sxs-lookup"><span data-stu-id="9d384-164">*MyHub.cs* will automatically open in the main window.</span></span> <span data-ttu-id="9d384-165">Sostituire il contenuto con il codice seguente, quindi salvare e chiudere il file:</span><span class="sxs-lookup"><span data-stu-id="9d384-165">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]
    
23. <span data-ttu-id="9d384-166">*[Crank.exe](signalr-connection-density-testing-with-crank.md)*  è una strumento fornito con la codebase SignalR di test della densità di connessione.</span><span class="sxs-lookup"><span data-stu-id="9d384-166">*[Crank.exe](signalr-connection-density-testing-with-crank.md)* is a connection density testing tool provided with the SignalR codebase.</span></span> <span data-ttu-id="9d384-167">Poiché il perno richiede una connessione permanente, si aggiungerne uno al sito per l'uso durante il test.</span><span class="sxs-lookup"><span data-stu-id="9d384-167">Since Crank requires a persistent connection, you add one to your site for use when testing.</span></span> <span data-ttu-id="9d384-168">Aggiungere una nuova cartella per il **WebRole1** progetto chiamato *PersistentConnections*.</span><span class="sxs-lookup"><span data-stu-id="9d384-168">Add a new folder to the **WebRole1** project called *PersistentConnections*.</span></span> <span data-ttu-id="9d384-169">Fare doppio clic su questa cartella e selezionare **Add** > **classe**.</span><span class="sxs-lookup"><span data-stu-id="9d384-169">Right-click this folder and select **Add** > **Class**.</span></span> <span data-ttu-id="9d384-170">Denominare il nuovo file di classe *MyPersistentConnections.cs* e selezionare **Add**.</span><span class="sxs-lookup"><span data-stu-id="9d384-170">Name the new class file *MyPersistentConnections.cs* and select **Add**.</span></span>

24. <span data-ttu-id="9d384-171">Visual Studio aprirà la *MyPersistentConnections.cs* file nella finestra principale.</span><span class="sxs-lookup"><span data-stu-id="9d384-171">Visual Studio will open the *MyPersistentConnections.cs* file in the main window.</span></span> <span data-ttu-id="9d384-172">Sostituire il contenuto con il codice seguente, quindi salvare e chiudere il file:</span><span class="sxs-lookup"><span data-stu-id="9d384-172">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]
    
25. <span data-ttu-id="9d384-173">Uso di `Startup` (classe), gli oggetti di SignalR avviano all'avvio di OWIN.</span><span class="sxs-lookup"><span data-stu-id="9d384-173">Using the `Startup` class, the SignalR objects start when OWIN starts up.</span></span> <span data-ttu-id="9d384-174">Aprire o creare *Startup.cs* e sostituire il contenuto con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="9d384-174">Open or create *Startup.cs* and replace the contents with the following code:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]
    
    <span data-ttu-id="9d384-175">Nel codice precedente, il `OwinStartup` attributo contrassegna questa classe per iniziare a OWIN.</span><span class="sxs-lookup"><span data-stu-id="9d384-175">In the code above, the `OwinStartup` attribute marks this class to start OWIN.</span></span> <span data-ttu-id="9d384-176">Il `Configuration` metodo inizia a SignalR.</span><span class="sxs-lookup"><span data-stu-id="9d384-176">The `Configuration` method starts SignalR.</span></span>
    
26. <span data-ttu-id="9d384-177">Test dell'applicazione nell'emulatore di Microsoft Azure premendo **F5**.</span><span class="sxs-lookup"><span data-stu-id="9d384-177">Test your application in the Microsoft Azure Emulator by pressing **F5**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9d384-178">Se si verifica una **FileLoadException** alla **MapSignalR**, modificare i reindirizzamenti di associazione nelle *Web. config* al seguente:</span><span class="sxs-lookup"><span data-stu-id="9d384-178">If you encounter a **FileLoadException** at **MapSignalR**, change the binding redirects in *web.config* to the following:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]
    
27. <span data-ttu-id="9d384-179">Attendere circa un minuto.</span><span class="sxs-lookup"><span data-stu-id="9d384-179">Wait about one minute.</span></span> <span data-ttu-id="9d384-180">Aprire la finestra degli strumenti di Cloud Explorer in Visual Studio (**View** > **Cloud Explorer**) ed espandere il percorso `(Local)/Storage Accounts/(Development)/Tables`.</span><span class="sxs-lookup"><span data-stu-id="9d384-180">Open the Cloud Explorer tool window in Visual Studio (**View** > **Cloud Explorer**) and expand the path `(Local)/Storage Accounts/(Development)/Tables`.</span></span> <span data-ttu-id="9d384-181">Fare doppio clic su **WADPerformanceCountersTable**.</span><span class="sxs-lookup"><span data-stu-id="9d384-181">Double-click **WADPerformanceCountersTable**.</span></span> <span data-ttu-id="9d384-182">Verranno visualizzati i contatori SignalR nei dati della tabella.</span><span class="sxs-lookup"><span data-stu-id="9d384-182">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="9d384-183">Se non viene visualizzata la tabella, si potrebbe essere necessario immettere nuovamente le credenziali di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="9d384-183">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="9d384-184">Potrebbe essere necessario selezionare la **Refresh** pulsante per visualizzare la tabella in **Cloud Explorer** oppure selezionare il **Aggiorna** pulsante nella finestra Apri tabella per visualizzare i dati nella tabella.</span><span class="sxs-lookup"><span data-stu-id="9d384-184">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

    ![Selezionare la tabella di contatori delle prestazioni di WAD in Visual Studio Cloud Explorer](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Che mostra i contatori raccolti nella tabella di contatori delle prestazioni di WAD](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)
    
28. <span data-ttu-id="9d384-187">Per testare l'applicazione nel cloud, aggiornare il **ServiceConfiguration** file e impostare il `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` su una stringa di connessione account di archiviazione di Azure valida.</span><span class="sxs-lookup"><span data-stu-id="9d384-187">To test your application in the cloud, update the **ServiceConfiguration.Cloud.cscfg** file and set the `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` to a valid Azure Storage account connection string.</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. <span data-ttu-id="9d384-188">Distribuire l'applicazione alla sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="9d384-188">Deploy the application to your Azure subscription.</span></span> <span data-ttu-id="9d384-189">Per informazioni dettagliate su come distribuire un'applicazione in Azure, vedere [come creare e distribuire un servizio Cloud](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="9d384-189">For details on how to deploy an application to Azure, see [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span>

30. <span data-ttu-id="9d384-190">Attendere alcuni minuti.</span><span class="sxs-lookup"><span data-stu-id="9d384-190">Wait a few minutes.</span></span> <span data-ttu-id="9d384-191">Nelle **Cloud Explorer**, individuare l'account di archiviazione configurata in precedenza e trovare il `WADPerformanceCountersTable` tabella in esso.</span><span class="sxs-lookup"><span data-stu-id="9d384-191">In **Cloud Explorer**, locate the storage account you configured above and find the `WADPerformanceCountersTable` table in it.</span></span> <span data-ttu-id="9d384-192">Verranno visualizzati i contatori SignalR nei dati della tabella.</span><span class="sxs-lookup"><span data-stu-id="9d384-192">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="9d384-193">Se non viene visualizzata la tabella, si potrebbe essere necessario immettere nuovamente le credenziali di archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="9d384-193">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="9d384-194">Potrebbe essere necessario selezionare la **Refresh** pulsante per visualizzare la tabella in **Cloud Explorer** oppure selezionare il **Aggiorna** pulsante nella finestra Apri tabella per visualizzare i dati nella tabella.</span><span class="sxs-lookup"><span data-stu-id="9d384-194">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

<span data-ttu-id="9d384-195">Ringraziamenti speciali [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) per visualizzarne il contenuto usato in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="9d384-195">Special thanks to [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) for the original content used in this tutorial.</span></span>
