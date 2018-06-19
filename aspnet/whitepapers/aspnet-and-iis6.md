---
uid: whitepapers/aspnet-and-iis6
title: Esecuzione di ASP.NET 1.1 con IIS 6.0 | Documenti Microsoft
author: rick-anderson
description: Sebbene Windows Server 2003 include sia IIS 6.0 e ASP.NET 1.1, questi componenti sono disabilitati per impostazione predefinita. Questo white paper descrive come abilitare IIS 6.0 un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 1fcac7b8bc295ccf4e36189295b6bc2e4d328623
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530110"
---
<a name="running-aspnet-11-with-iis-60"></a><span data-ttu-id="6b74c-104">Esecuzione di ASP.NET 1.1 con IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="6b74c-104">Running ASP.NET 1.1 with IIS 6.0</span></span>
====================
> <span data-ttu-id="6b74c-105">Sebbene Windows Server 2003 include sia IIS 6.0 e ASP.NET 1.1, questi componenti sono disabilitati per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="6b74c-105">While Windows Server 2003 includes both IIS 6.0 and ASP.NET 1.1, these components are disabled by default.</span></span> <span data-ttu-id="6b74c-106">Questo white paper viene descritto come abilitare IIS 6.0 e ASP.NET 1.1 e consiglia diverse impostazioni di configurazione per ottenere prestazioni ottimali da IIS e ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6b74c-106">This whitepaper describes how to enable IIS 6.0 and ASP.NET 1.1, and recommends several configuration settings to get the optimal performance from IIS and ASP.NET.</span></span>
> 
> <span data-ttu-id="6b74c-107">Si applica a ASP.NET 1.1 e IIS 6.0.</span><span class="sxs-lookup"><span data-stu-id="6b74c-107">Applies to ASP.NET 1.1 and IIS 6.0.</span></span>


<span data-ttu-id="6b74c-108">ASP.NET 1.1 viene fornito con Windows Server 2003, che include anche la versione più recente di Internet Information Server (IIS) versione 6.0.</span><span class="sxs-lookup"><span data-stu-id="6b74c-108">ASP.NET 1.1 ships with Windows Server 2003, which also includes the latest version of Internet Information Server (IIS) version 6.0.</span></span> <span data-ttu-id="6b74c-109">IIS 6.0 e ASP.NET 1.1 sono progettati per integrarsi perfettamente e ASP.NET viene ora impostato per il nuovo modello di processo di lavoro di IIS 6.0.</span><span class="sxs-lookup"><span data-stu-id="6b74c-109">IIS 6.0 and ASP.NET 1.1 are designed to integrate seamlessly and ASP.NET now defaults to the new IIS 6.0 worker process model.</span></span>

## <a name="aspnet-11-is-not-installed-by-default"></a><span data-ttu-id="6b74c-110">ASP.NET 1.1 non è installato per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="6b74c-110">ASP.NET 1.1 is not installed by default</span></span>

<span data-ttu-id="6b74c-111">Diversamente dalle versioni precedenti di sistemi operativi Microsoft, Internet Information Server (IIS) non è abilitato per impostazione predefinita. non è ASP.NET 1.1.</span><span class="sxs-lookup"><span data-stu-id="6b74c-111">Unlike previous versions of Microsoft's server operating systems, Internet Information Server (IIS) is not enabled by default; nor is ASP.NET 1.1.</span></span> <span data-ttu-id="6b74c-112">Sono disponibili due opzioni per l'abilitazione di IIS:</span><span class="sxs-lookup"><span data-stu-id="6b74c-112">There are two options for enabling IIS:</span></span>

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a><span data-ttu-id="6b74c-113">L'abilitazione di IIS, l'opzione #1 - Configurazione guidata Server</span><span class="sxs-lookup"><span data-stu-id="6b74c-113">Enabling IIS, option #1 - Configure Your Server Wizard</span></span>

<span data-ttu-id="6b74c-114">Windows Server 2003 è disponibile un nuovo 'Configurazione guidata Server' che consentono di configurare correttamente il server in modalità desiderata.</span><span class="sxs-lookup"><span data-stu-id="6b74c-114">Windows Server 2003 ships a new 'Configure Your Server Wizard' to help you properly configure your server in the desired mode.</span></span>

<span data-ttu-id="6b74c-115">Per avviare la procedura guidata - nota, eseguire la procedura guidata è necessario eseguire l'accesso come amministratore, passare a: Start | I programmi | Strumenti di amministrazione e selezionare 'configurazione Server".</span><span class="sxs-lookup"><span data-stu-id="6b74c-115">To start the wizard - note, to run the wizard you must be logged in as an administrator - go to: Start | Programs | Administrative Tools and select 'Configure Your Server'.</span></span>

<span data-ttu-id="6b74c-116">Una volta selezionata verrà visualizzata la schermata di apertura 'Configurazione guidata Server':</span><span class="sxs-lookup"><span data-stu-id="6b74c-116">Once selected you should see the 'Configure Your Server Wizard' opening screen:</span></span>

![](aspnet-and-iis6/_static/image1.jpg)

<span data-ttu-id="6b74c-117">Fare clic su ' Avanti &gt;':</span><span class="sxs-lookup"><span data-stu-id="6b74c-117">Click 'Next &gt;':</span></span>

![](aspnet-and-iis6/_static/image2.jpg)

<span data-ttu-id="6b74c-118">Fare clic su ' Avanti &gt;'</span><span class="sxs-lookup"><span data-stu-id="6b74c-118">Click 'Next &gt;'</span></span>

![](aspnet-and-iis6/_static/image3.jpg)

<span data-ttu-id="6b74c-119">In questa schermata sarà necessario selezionare "il server applicazioni (IIS, ASP.NET) come opzioni di configurazione.</span><span class="sxs-lookup"><span data-stu-id="6b74c-119">On this screen you will need to select 'Application server (IIS, ASP.NET) as the options to configure.</span></span>

<span data-ttu-id="6b74c-120">Fare clic su ' Avanti &gt;'.</span><span class="sxs-lookup"><span data-stu-id="6b74c-120">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image4.jpg)

<span data-ttu-id="6b74c-121">Dopo aver selezionato per configurare il server come Server applicazioni, verrà visualizzata questa schermata che richiede le funzionalità aggiuntive devono essere installate.</span><span class="sxs-lookup"><span data-stu-id="6b74c-121">After selecting to configure the server as an Application Server, this screen will be displayed prompting what additional capabilities should be installed.</span></span> <span data-ttu-id="6b74c-122">Nessuna opzione è selezionata per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="6b74c-122">Neither option is selected by default.</span></span> <span data-ttu-id="6b74c-123">Per abilitare ASP.NET automaticamente, è necessario selezionare ' abilitare ASP. NET'.</span><span class="sxs-lookup"><span data-stu-id="6b74c-123">To enable ASP.NET automatically, you need to select 'Enable ASP.NET'.</span></span>

<span data-ttu-id="6b74c-124">Fare clic su ' Avanti &gt;'.</span><span class="sxs-lookup"><span data-stu-id="6b74c-124">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image5.jpg)

<span data-ttu-id="6b74c-125">Questa schermata consente di visualizzare che cosa sono le opzioni da installare.</span><span class="sxs-lookup"><span data-stu-id="6b74c-125">This screen displays what options are to be installed.</span></span>

<span data-ttu-id="6b74c-126">Fare clic su ' Avanti &gt;'.</span><span class="sxs-lookup"><span data-stu-id="6b74c-126">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image6.jpg)

<span data-ttu-id="6b74c-127">Verrà visualizzata questa schermata durante l'installazione le opzioni selezionate.</span><span class="sxs-lookup"><span data-stu-id="6b74c-127">You will see this screen while the options you selected are being installed.</span></span> <span data-ttu-id="6b74c-128">È normale per visualizzare altri finestra di dialogo verranno visualizzate finestre di come vengono installati i servizi.</span><span class="sxs-lookup"><span data-stu-id="6b74c-128">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="6b74c-129">Potrebbe inoltre essere richiesto per la posizione del CD di installazione di Windows Server 2003.</span><span class="sxs-lookup"><span data-stu-id="6b74c-129">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="6b74c-130">Fare clic su ' Avanti &gt;' al termine.</span><span class="sxs-lookup"><span data-stu-id="6b74c-130">Click 'Next &gt;' when complete.</span></span>

![](aspnet-and-iis6/_static/image7.jpg)

<span data-ttu-id="6b74c-131">Fare clic su 'Fine' - Windows Server 2003 è ora configurato per il supporto di IIS 6.0 e ASP.NET 1.1.</span><span class="sxs-lookup"><span data-stu-id="6b74c-131">Click 'Finish' - the Windows Server 2003 is now configured to support IIS 6.0 and ASP.NET 1.1.</span></span>

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a><span data-ttu-id="6b74c-132">L'attivazione di IIS, l'opzione #2: configurare manualmente IIS e ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6b74c-132">Enabling IIS, option #2 - Manually configuring IIS and ASP.NET</span></span>

<span data-ttu-id="6b74c-133">Se non si desidera utilizzare il "Configurazione guidata Server' è facoltativamente possibile installare IIS 6.0 e ASP.NET 1.1 tramite 'Installazione applicazioni' nel Pannello di controllo.</span><span class="sxs-lookup"><span data-stu-id="6b74c-133">If you do not wish to use the 'Configure Your Server Wizard' you can optionally install IIS 6.0 and ASP.NET 1.1 using 'Add or Remove Programs' from the Control Panel.</span></span>

<span data-ttu-id="6b74c-134">Innanzitutto, aprire il pannello di controllo:</span><span class="sxs-lookup"><span data-stu-id="6b74c-134">First open the Control Panel:</span></span>

![](aspnet-and-iis6/_static/image8.jpg)

<span data-ttu-id="6b74c-135">Successivamente, fare clic su ' Aggiungi/Rimuovi componenti di Windows' verrà aperta 'Guidata componenti di Windows':</span><span class="sxs-lookup"><span data-stu-id="6b74c-135">Next, click on 'Add/Remove Windows Components' which will open the 'Windows Components Wizard':</span></span>

![](aspnet-and-iis6/_static/image9.jpg)

<span data-ttu-id="6b74c-136">Evidenziare e selezionare 'Server di applicazioni' e quindi fare clic su "Dettagli"?</span><span class="sxs-lookup"><span data-stu-id="6b74c-136">Highlight and check 'Application Server' and then click the 'Details?'</span></span> <span data-ttu-id="6b74c-137">pulsante:</span><span class="sxs-lookup"><span data-stu-id="6b74c-137">button:</span></span>

![](aspnet-and-iis6/_static/image10.jpg)

<span data-ttu-id="6b74c-138">Per installare ASP.NET, selezionare ' ASP. NET'.</span><span class="sxs-lookup"><span data-stu-id="6b74c-138">To install ASP.NET, check 'ASP.NET'.</span></span>

<span data-ttu-id="6b74c-139">Fare clic su 'OK' per tornare alla finestra di aggiunta guidata componenti di Windows.</span><span class="sxs-lookup"><span data-stu-id="6b74c-139">Click 'OK' to return to the Windows Component Wizard.</span></span> <span data-ttu-id="6b74c-140">Fare clic su ' Avanti &gt;' dall'Aggiunta guidata componenti di Windows per avviare l'installazione:</span><span class="sxs-lookup"><span data-stu-id="6b74c-140">Click 'Next &gt;' from the Windows Component Wizard to begin installing:</span></span>

![](aspnet-and-iis6/_static/image11.jpg)

<span data-ttu-id="6b74c-141">È normale per visualizzare altri finestra di dialogo verranno visualizzate finestre di come vengono installati i servizi.</span><span class="sxs-lookup"><span data-stu-id="6b74c-141">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="6b74c-142">Potrebbe inoltre essere richiesto per la posizione del CD di installazione di Windows Server 2003.</span><span class="sxs-lookup"><span data-stu-id="6b74c-142">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="6b74c-143">Al termine dell'installazione verrà visualizzata la schermata ultimo dell'Aggiunta guidata componenti di Windows:</span><span class="sxs-lookup"><span data-stu-id="6b74c-143">When installation is complete you will see the last screen of the Windows Component Wizard:</span></span>

![](aspnet-and-iis6/_static/image12.jpg)

<span data-ttu-id="6b74c-144">IIS 6.0 e ASP.NET 1.1 siano configurati e disponibili.</span><span class="sxs-lookup"><span data-stu-id="6b74c-144">IIS 6.0 and ASP.NET 1.1 are now configured and available.</span></span>

## <a name="recommended-settings"></a><span data-ttu-id="6b74c-145">Impostazioni consigliate</span><span class="sxs-lookup"><span data-stu-id="6b74c-145">Recommended Settings</span></span>

<span data-ttu-id="6b74c-146">Durante l'esecuzione di ASP.NET 1.1 con IIS 6.0 sono disponibili diverse impostazioni di configurazione consigliati per ottenere prestazioni ottimali da ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="6b74c-146">When running ASP.NET 1.1 with IIS 6.0 there are several configuration settings that are recommended to get the optimal performance from ASP.NET:</span></span>

- <span data-ttu-id="6b74c-147">Configurazione dei limiti di memoria processo di lavoro</span><span class="sxs-lookup"><span data-stu-id="6b74c-147">Configuring worker process memory limits</span></span>
- <span data-ttu-id="6b74c-148">Il riciclo del processo di configurazione</span><span class="sxs-lookup"><span data-stu-id="6b74c-148">Configuring worker process recycling</span></span>

### <a name="configuring-worker-process-memory-limits"></a><span data-ttu-id="6b74c-149">Configurazione dei limiti di memoria processo di lavoro</span><span class="sxs-lookup"><span data-stu-id="6b74c-149">Configuring worker process memory limits</span></span>

<span data-ttu-id="6b74c-150">Per impostazione predefinita, IIS 6.0 non impostare un limite sulla quantità di memoria che IIS è possibile utilizzare.</span><span class="sxs-lookup"><span data-stu-id="6b74c-150">By default IIS 6.0 does not set a limit on the amount of memory that IIS is allowed to use.</span></span> <span data-ttu-id="6b74c-151">ASP. Funzionalità della Cache della rete si basa su un limite di memoria Cache potrà così in modo proattivo rimuovere elementi inutilizzati dalla memoria.</span><span class="sxs-lookup"><span data-stu-id="6b74c-151">ASP.NET's Cache feature relies on a limitation of memory so the Cache can proactively remove unused items from memory.</span></span>

<span data-ttu-id="6b74c-152">Si consiglia di configurare la memoria, il riciclo delle funzionalità di IIS 6.0.</span><span class="sxs-lookup"><span data-stu-id="6b74c-152">It is recommended that you configure the memory recycling feature of IIS 6.0.</span></span> <span data-ttu-id="6b74c-153">Per configurare questo aprire Gestione Internet Information Services (Start | I programmi | Strumenti di amministrazione | Internet Information Services).</span><span class="sxs-lookup"><span data-stu-id="6b74c-153">To configure this open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="6b74c-154">Una volta aperto, espandere la cartella 'Pool di applicazioni':</span><span class="sxs-lookup"><span data-stu-id="6b74c-154">Once open, expand the 'Application Pools' folder:</span></span>

<span data-ttu-id="6b74c-155">Per ogni pool di applicazioni:</span><span class="sxs-lookup"><span data-stu-id="6b74c-155">For each application pool:</span></span>

![](aspnet-and-iis6/_static/image13.jpg)

1. <span data-ttu-id="6b74c-156">Pulsante destro del mouse sul pool di applicazioni, ad esempio 'DefaultAppPool' e 'Properties' selezionare:</span><span class="sxs-lookup"><span data-stu-id="6b74c-156">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image14.jpg)

2. <span data-ttu-id="6b74c-157">Successivamente, abilitare il riciclo memoria facendo clic su uno ' quantità massima di memoria utilizzata (in megabyte):'.</span><span class="sxs-lookup"><span data-stu-id="6b74c-157">Next, enable Memory recycling by clicking on either 'Maximum used memory (in megabytes):'.</span></span> <span data-ttu-id="6b74c-158">Il valore non deve essere maggiore di memoria fisica (non virtuale) nel server, una buona approssimazione è 60% della memoria fisica, ad esempio per un server con 512MB di memoria fisica, selezionare 310.</span><span class="sxs-lookup"><span data-stu-id="6b74c-158">The value should not be more than the amount of physical (not virtual) memory on the server, a good approximation is 60% of the physical memory, i.e. for a server with 512MB of physical memory select 310.</span></span> <span data-ttu-id="6b74c-159">È inoltre consigliabile che il valore massimo non superi 800MB quando si utilizza uno spazio degli indirizzi di 2GB.</span><span class="sxs-lookup"><span data-stu-id="6b74c-159">It is also recommended that the maximum not exceed 800MB when using a 2GB address space.</span></span> <span data-ttu-id="6b74c-160">Se lo spazio degli indirizzi di memoria del server è di 3GB, il limite di memoria massima per il processo di lavoro può essere a un massimo di 1, 800MB:</span><span class="sxs-lookup"><span data-stu-id="6b74c-160">If the memory address space of the server is 3GB, the maximum memory limit for the worker process can be as high as 1,800MB:</span></span>

![](aspnet-and-iis6/_static/image15.jpg)

<span data-ttu-id="6b74c-161">Fare clic su 'Applica' e 'OK' per uscire dalla finestra di dialogo proprietà.</span><span class="sxs-lookup"><span data-stu-id="6b74c-161">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="6b74c-162">Ripetere questo passaggio per tutti i pool di applicazioni disponibili.</span><span class="sxs-lookup"><span data-stu-id="6b74c-162">Repeat this for all available application pools.</span></span>

### <a name="configuring-worker-recycling"></a><span data-ttu-id="6b74c-163">Configurazione riciclo di lavoro</span><span class="sxs-lookup"><span data-stu-id="6b74c-163">Configuring worker recycling</span></span>

<span data-ttu-id="6b74c-164">Per impostazione predefinita IIS 6.0 è configurato per riciclare il processo di lavoro 29 ore.</span><span class="sxs-lookup"><span data-stu-id="6b74c-164">By default IIS 6.0 is configured to recycle its worker process every 29 hours.</span></span> <span data-ttu-id="6b74c-165">Si tratta di un bit aggressiva per un'applicazione in esecuzione ASP.NET e, è consigliabile che il riciclo dei processi di lavoro automatico è disabilitato.</span><span class="sxs-lookup"><span data-stu-id="6b74c-165">This is a bit aggressive for an application running ASP.NET and it is recommended that automatic worker process recycling is disabled.</span></span>

<span data-ttu-id="6b74c-166">Per disabilitare il riciclo dei processi di lavoro automatico, innanzitutto aprire Gestione Internet Information Services (Start | I programmi | Strumenti di amministrazione | Internet Information Services).</span><span class="sxs-lookup"><span data-stu-id="6b74c-166">To disable automatic worker process recycling, first open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="6b74c-167">Una volta aperto, espandere la cartella 'Pool di applicazioni':</span><span class="sxs-lookup"><span data-stu-id="6b74c-167">Once open, expand the 'Application Pools' folder:</span></span>

![](aspnet-and-iis6/_static/image16.jpg)

<span data-ttu-id="6b74c-168">Per ogni pool di applicazioni:</span><span class="sxs-lookup"><span data-stu-id="6b74c-168">For each application pool:</span></span>

1. <span data-ttu-id="6b74c-169">Pulsante destro del mouse sul pool di applicazioni, ad esempio 'DefaultAppPool' e 'Properties' selezionare:</span><span class="sxs-lookup"><span data-stu-id="6b74c-169">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image17.jpg)

2. <span data-ttu-id="6b74c-170">Deselezionare l'opzione ' Ricicla il processo (in minuti):':</span><span class="sxs-lookup"><span data-stu-id="6b74c-170">Uncheck 'Recycle worker process (in minutes):':</span></span>

![](aspnet-and-iis6/_static/image18.jpg)

<span data-ttu-id="6b74c-171">Fare clic su 'Applica' e 'OK' per uscire dalla finestra di dialogo proprietà.</span><span class="sxs-lookup"><span data-stu-id="6b74c-171">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="6b74c-172">Ripetere questo passaggio per tutti i pool di applicazioni disponibili.</span><span class="sxs-lookup"><span data-stu-id="6b74c-172">Repeat this for all available application pools.</span></span>

## <a name="granting-write-access-to-the-file-system"></a><span data-ttu-id="6b74c-173">Concessione dell'accesso in scrittura nel file System</span><span class="sxs-lookup"><span data-stu-id="6b74c-173">Granting write access to the file system</span></span>

<span data-ttu-id="6b74c-174">Se si utilizza NTFS, è necessario modificare l'elenco di controllo un accesso (ACL) per il file o una cartella per concedere l'accesso ASP.NET per l'applicazione richiede l'accesso in scrittura nel file System.</span><span class="sxs-lookup"><span data-stu-id="6b74c-174">If your application requires write access to the file system and you are using NTFS you will need to modify an Access Control List (ACL) on the folder or file to grant ASP.NET access to.</span></span>

<span data-ttu-id="6b74c-175">Ad esempio, per concedere ASP.NET accesso in scrittura per il c:\inetpub\wwwroot innanzitutto aprire Esplora e passare alla directory:</span><span class="sxs-lookup"><span data-stu-id="6b74c-175">For example, to grant ASP.NET write access to the c:\inetpub\wwwroot first open explorer and navigate to the directory:</span></span>

![](aspnet-and-iis6/_static/image19.jpg)

<span data-ttu-id="6b74c-176">Successivamente, fare doppio clic sulla directory, ad esempio 'wwwroot' e scegliere Proprietà.</span><span class="sxs-lookup"><span data-stu-id="6b74c-176">Next, right-click on the directory, e.g. 'wwwroot' and select properties.</span></span> <span data-ttu-id="6b74c-177">Una volta aperta la finestra di dialogo proprietà, selezionare la scheda 'Security':</span><span class="sxs-lookup"><span data-stu-id="6b74c-177">After the properties dialog opens, select the 'Security' tab:</span></span>

![](aspnet-and-iis6/_static/image20.jpg)

<span data-ttu-id="6b74c-178">La directory c:\inetpub\wwwroot\ è una directory speciale in cui il gruppo di IIS 6.0 speciale ' IIS\_WPG' è già stata concessa lettura &amp; le autorizzazioni di esecuzione, visualizzazione contenuto cartella e lettura.</span><span class="sxs-lookup"><span data-stu-id="6b74c-178">The c:\inetpub\wwwroot\ directory is a special directory in that the special IIS 6.0 group 'IIS\_WPG' is already granted Read &amp; Execute, List Folder Contents, and Read permissions.</span></span> <span data-ttu-id="6b74c-179">Tuttavia, per concedere l'autorizzazione di scrittura, è necessario selezionare la casella di controllo Consenti per la scrittura:</span><span class="sxs-lookup"><span data-stu-id="6b74c-179">However, to grant Write permission, you need to click the Allow checkbox for Write:</span></span>

![](aspnet-and-iis6/_static/image21.jpg)

<span data-ttu-id="6b74c-180">IIS 6.0 include l'autorizzazione di scrittura per questa cartella.</span><span class="sxs-lookup"><span data-stu-id="6b74c-180">IIS 6.0 now has write permission on this folder.</span></span> <span data-ttu-id="6b74c-181">Per concedere autorizzazioni di scrittura in altre cartelle, eseguire la procedura, si noti che potrebbe essere necessario aggiungere IIS\_gruppo WPG se non esiste già.</span><span class="sxs-lookup"><span data-stu-id="6b74c-181">To grant write permissions on other folders, follow these steps - note, you may need to add the IIS\_WPG group if it does not already exist.</span></span>

> [!CAUTION]
> <span data-ttu-id="6b74c-182">Concessione dell'autorizzazione di scrittura a IIS\_WPG consentirà di qualsiasi applicazione ASP.NET scrivere in questa directory.</span><span class="sxs-lookup"><span data-stu-id="6b74c-182">Granting write permission to IIS\_WPG will allow any ASP.NET application to write to this directory.</span></span>

## <a name="supporting-integrated-authentication-with-sql-server"></a><span data-ttu-id="6b74c-183">Supportano l'autenticazione integrata con SQL Server</span><span class="sxs-lookup"><span data-stu-id="6b74c-183">Supporting integrated authentication with SQL Server</span></span>

<span data-ttu-id="6b74c-184">L'autenticazione integrata consente di sfruttare l'autenticazione di Windows NT di SQL Server per convalidare gli account di accesso di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="6b74c-184">Integrated authentication allows for SQL Server to leverage Windows NT authentication to validate SQL Server logon accounts.</span></span> <span data-ttu-id="6b74c-185">Ciò consente all'utente di ignorare il processo di accesso di SQL Server standard.</span><span class="sxs-lookup"><span data-stu-id="6b74c-185">This allows the user to bypass the standard SQL Server logon process.</span></span> <span data-ttu-id="6b74c-186">Con questo approccio, un utente di rete possa accedere a un database di SQL Server senza fornire un ID di accesso separato o una password, in quanto SQL Server Ottiene le informazioni sull'utente e password dal processo di sicurezza di rete di Windows NT.</span><span class="sxs-lookup"><span data-stu-id="6b74c-186">With this approach, a network user can access a SQL Server database without supplying a separate logon identification or password because SQL Server obtains the user and password information from the Windows NT network security process.</span></span>

<span data-ttu-id="6b74c-187">Scelta dell'autenticazione integrata per le applicazioni ASP.NET è una buona scelta poiché credenziali non vengono mai archiviate all'interno della stringa di connessione per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="6b74c-187">Choosing integrated authentication for ASP.NET applications is a good choice because no credentials are ever stored within your connection string for your application.</span></span> <span data-ttu-id="6b74c-188">Anziché la stringa di connessione utilizzata per connettersi a SQL apparirà come segue:</span><span class="sxs-lookup"><span data-stu-id="6b74c-188">Rather the connection string used to connect to SQL will look as follows:</span></span>

`"server=localhost; database=Northwind;Trusted_Connection=true"`

<span data-ttu-id="6b74c-189">La stringa di connessione al Server di SQL per utilizzare le credenziali di Windows dell'applicazione, il tentativo di accedere a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="6b74c-189">This connection string tells SQL Server to use the Windows credentials of the application attempting to access SQL Server.</span></span> <span data-ttu-id="6b74c-190">Nel caso di ASP.NET/IIS 6 questo sarebbe un account in IIS\_gruppo WPG.</span><span class="sxs-lookup"><span data-stu-id="6b74c-190">In the case of ASP.NET/IIS 6 this would be an account in the IIS\_WPG group.</span></span>

<span data-ttu-id="6b74c-191">Per abilitare l'autenticazione integrata tra ASP.NET e SQL Server, è necessario innanzitutto verificare che SQL Server sia configurato per l'autenticazione integrata oppure l'autenticazione in modalità mista, verificare con l'amministratore del database a tale scopo.</span><span class="sxs-lookup"><span data-stu-id="6b74c-191">To enable integrated authentication between SQL Server and ASP.NET, you will need to first ensure that SQL Server is configured for either Integrated authentication or Mixed-Mode authentication - check with your DBA to determine this.</span></span> <span data-ttu-id="6b74c-192">Se SQL Server è in uno di questi due modalità, è possibile utilizzare l'autenticazione integrata.</span><span class="sxs-lookup"><span data-stu-id="6b74c-192">If SQL Server is in one of these two modes, you can use integrated authentication.</span></span>

<span data-ttu-id="6b74c-193">Aprire SQL Server Enterprise Manager (Start | I programmi | Microsoft SQL Server | Enterprise Manager), selezionare il server appropriato ed espandere la cartella sicurezza:</span><span class="sxs-lookup"><span data-stu-id="6b74c-193">Open SQL Server Enterprise Manager (Start | Programs | Microsoft SQL Server | Enterprise Manager), select the appropriate server, and expand the Security folder:</span></span>

![](aspnet-and-iis6/_static/image22.jpg)

<span data-ttu-id="6b74c-194">Se ' BUILTINT\IIS\_WPG' gruppo non è elencato, fare clic su account di accesso e selezionare 'Nuovo account di accesso':</span><span class="sxs-lookup"><span data-stu-id="6b74c-194">If 'BUILTINT\IIS\_WPG' group is not listed, right-click on Logins and select 'New Login':</span></span>

![](aspnet-and-iis6/_static/image23.jpg)

<span data-ttu-id="6b74c-195">Nel ' nome:' casella di testo immettere ' [nome Server o dominio] \IIS\_WPG' o fare clic sul pulsante dei puntini di sospensione per aprire il selettore di utente/gruppo di Windows NT:</span><span class="sxs-lookup"><span data-stu-id="6b74c-195">In the 'Name:' textbox either enter '[Server/Domain Name]\IIS\_WPG' or click on the ellipses button to open the Windows NT user/group picker:</span></span>

![](aspnet-and-iis6/_static/image24.jpg)

<span data-ttu-id="6b74c-196">Selezionare IIS corrente del computer\_gruppo WPG e fare clic su 'Add' e su OK per chiudere il selettore.</span><span class="sxs-lookup"><span data-stu-id="6b74c-196">Select the current machine's IIS\_WPG group and click 'Add' and OK to close the picker.</span></span>

<span data-ttu-id="6b74c-197">È quindi necessario impostare anche il database predefinito e le autorizzazioni per accedere al database.</span><span class="sxs-lookup"><span data-stu-id="6b74c-197">You then need to also set the default database and the permissions to access the database.</span></span> <span data-ttu-id="6b74c-198">Per impostare il database predefinito scegliere dall'elenco a discesa è selezionata, ad esempio Northwind riportato di seguito:</span><span class="sxs-lookup"><span data-stu-id="6b74c-198">To set the default database choose from the drop down list, e.g. below Northwind is selected:</span></span>

![](aspnet-and-iis6/_static/image25.jpg)

<span data-ttu-id="6b74c-199">Successivamente, fare clic sulla scheda di accesso al Database:</span><span class="sxs-lookup"><span data-stu-id="6b74c-199">Next, click on the Database Access tab:</span></span>

![](aspnet-and-iis6/_static/image26.jpg)

<span data-ttu-id="6b74c-200">Fare clic sulla casella di controllo Consenti per ogni database che si desidera consentire l'accesso a.</span><span class="sxs-lookup"><span data-stu-id="6b74c-200">Click on the Permit checkbox for every database that you wish to allow access to.</span></span> <span data-ttu-id="6b74c-201">È inoltre necessario selezionare i ruoli del database, controllo db\_proprietario garantisce l'account di accesso ha tutte le autorizzazioni necessarie per gestire e utilizzare il database selezionato.</span><span class="sxs-lookup"><span data-stu-id="6b74c-201">You will also need to select database roles, checking db\_owner will ensure your login has all necessary permissions to manage and use the selected database.</span></span>

<span data-ttu-id="6b74c-202">Fare clic su OK per chiudere la finestra di dialogo proprietà.</span><span class="sxs-lookup"><span data-stu-id="6b74c-202">Click OK to exit the property dialog.</span></span> <span data-ttu-id="6b74c-203">L'applicazione ASP.NET è ora configurato per supportare l'autenticazione integrata di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="6b74c-203">Your ASP.NET application is now configured to support integrated SQL Server authentication.</span></span>

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a><span data-ttu-id="6b74c-204">Versione 1.0 di ASP.NET non vengono eseguiti in modalità nativa di IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="6b74c-204">Don't run ASP.NET 1.0 in IIS 6.0 native mode</span></span>

<span data-ttu-id="6b74c-205">1.0 di ASP.NET in IIS 6.0 è supportata solo in modalità di compatibilità di IIS 5.</span><span class="sxs-lookup"><span data-stu-id="6b74c-205">ASP.NET 1.0 on IIS 6.0 is only supported in IIS 5 compatibility mode.</span></span>

<span data-ttu-id="6b74c-206">Per configurare ASP.NET 1.0 per l'esecuzione in modalità compatibilità IIS 5.0, aprire Gestione servizi Internet e fare clic con il pulsante destro siti Web e selezionare le proprietà:</span><span class="sxs-lookup"><span data-stu-id="6b74c-206">To configure ASP.NET 1.0 to run in IIS 5.0 compatibility mode, open Internet Services Manager and right click Web Sites and select properties:</span></span>

![](aspnet-and-iis6/_static/image27.jpg)

<span data-ttu-id="6b74c-207">Passare alla scheda servizio e controllare? Esegui il servizio WWW in modalità isolamento IIS 5.0:</span><span class="sxs-lookup"><span data-stu-id="6b74c-207">Switch to the Service Tab and check ?Run WWW Service in IIS 5.0 Isolation Mode?:</span></span>

![](aspnet-and-iis6/_static/image28.jpg)
