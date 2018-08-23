---
uid: whitepapers/aspnet-and-iis6
title: Esecuzione di ASP.NET 1.1 con IIS 6.0 | Microsoft Docs
author: rick-anderson
description: Mentre Windows Server 2003 include sia IIS 6.0 e ASP.NET 1.1, questi componenti sono disabilitati per impostazione predefinita. Questo white paper viene descritto come abilitare IIS 6.0 un...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 38cd0abc1e9133b9b86cff6dd2759ce98ac5a115
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838969"
---
<a name="running-aspnet-11-with-iis-60"></a><span data-ttu-id="65b91-104">Esecuzione di ASP.NET 1.1 con IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="65b91-104">Running ASP.NET 1.1 with IIS 6.0</span></span>
====================
> <span data-ttu-id="65b91-105">Mentre Windows Server 2003 include sia IIS 6.0 e ASP.NET 1.1, questi componenti sono disabilitati per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="65b91-105">While Windows Server 2003 includes both IIS 6.0 and ASP.NET 1.1, these components are disabled by default.</span></span> <span data-ttu-id="65b91-106">Questo white paper viene descritto come abilitare IIS 6.0 e ASP.NET 1.1 e consiglia di diverse impostazioni di configurazione per ottenere prestazioni ottimali da IIS e ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="65b91-106">This whitepaper describes how to enable IIS 6.0 and ASP.NET 1.1, and recommends several configuration settings to get the optimal performance from IIS and ASP.NET.</span></span>
> 
> <span data-ttu-id="65b91-107">Si applica a IIS 6.0 e ASP.NET 1.1.</span><span class="sxs-lookup"><span data-stu-id="65b91-107">Applies to ASP.NET 1.1 and IIS 6.0.</span></span>


<span data-ttu-id="65b91-108">In ASP.NET 1.1 viene fornito con Windows Server 2003, che include anche la versione più recente di Internet Information Server (IIS) versione 6.0.</span><span class="sxs-lookup"><span data-stu-id="65b91-108">ASP.NET 1.1 ships with Windows Server 2003, which also includes the latest version of Internet Information Server (IIS) version 6.0.</span></span> <span data-ttu-id="65b91-109">IIS 6.0 e ASP.NET 1.1 sono progettati per integrarsi perfettamente e ASP.NET ora impostazioni predefinite per il nuovo modello di processo di lavoro di IIS 6.0.</span><span class="sxs-lookup"><span data-stu-id="65b91-109">IIS 6.0 and ASP.NET 1.1 are designed to integrate seamlessly and ASP.NET now defaults to the new IIS 6.0 worker process model.</span></span>

## <a name="aspnet-11-is-not-installed-by-default"></a><span data-ttu-id="65b91-110">In ASP.NET 1.1 non è installato per impostazione predefinita</span><span class="sxs-lookup"><span data-stu-id="65b91-110">ASP.NET 1.1 is not installed by default</span></span>

<span data-ttu-id="65b91-111">Diversamente dalle versioni precedenti dei sistemi operativi server di Microsoft, Internet Information Server (IIS) non è abilitato per impostazione predefinita. né ASP.NET 1.1.</span><span class="sxs-lookup"><span data-stu-id="65b91-111">Unlike previous versions of Microsoft's server operating systems, Internet Information Server (IIS) is not enabled by default; nor is ASP.NET 1.1.</span></span> <span data-ttu-id="65b91-112">Sono disponibili due opzioni per l'abilitazione di IIS:</span><span class="sxs-lookup"><span data-stu-id="65b91-112">There are two options for enabling IIS:</span></span>

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a><span data-ttu-id="65b91-113">Abilitazione della configurazione guidata Server IIS, #1 - opzione</span><span class="sxs-lookup"><span data-stu-id="65b91-113">Enabling IIS, option #1 - Configure Your Server Wizard</span></span>

<span data-ttu-id="65b91-114">Windows Server 2003 viene fornito un nuovo 'Configurazione guidata Server"che consentono di configurare correttamente il server in modalità desiderata.</span><span class="sxs-lookup"><span data-stu-id="65b91-114">Windows Server 2003 ships a new 'Configure Your Server Wizard' to help you properly configure your server in the desired mode.</span></span>

<span data-ttu-id="65b91-115">Per avviare la procedura guidata: passare a nota, eseguire la procedura guidata è necessario essere l'accesso come amministratore:: Start | I programmi | Strumenti di amministrazione e selezionare "Configurazione guidata Server'.</span><span class="sxs-lookup"><span data-stu-id="65b91-115">To start the wizard - note, to run the wizard you must be logged in as an administrator - go to: Start | Programs | Administrative Tools and select 'Configure Your Server'.</span></span>

<span data-ttu-id="65b91-116">Una volta selezionata visualizzata la schermata di apertura 'Configurazione guidata Server':</span><span class="sxs-lookup"><span data-stu-id="65b91-116">Once selected you should see the 'Configure Your Server Wizard' opening screen:</span></span>

![](aspnet-and-iis6/_static/image1.jpg)

<span data-ttu-id="65b91-117">Fare clic su ' Avanti &gt;':</span><span class="sxs-lookup"><span data-stu-id="65b91-117">Click 'Next &gt;':</span></span>

![](aspnet-and-iis6/_static/image2.jpg)

<span data-ttu-id="65b91-118">Fare clic su ' Avanti &gt;'</span><span class="sxs-lookup"><span data-stu-id="65b91-118">Click 'Next &gt;'</span></span>

![](aspnet-and-iis6/_static/image3.jpg)

<span data-ttu-id="65b91-119">In questa schermata sarà necessario selezionare ' server di applicazioni (IIS, ASP.NET) come opzioni per la configurazione.</span><span class="sxs-lookup"><span data-stu-id="65b91-119">On this screen you will need to select 'Application server (IIS, ASP.NET) as the options to configure.</span></span>

<span data-ttu-id="65b91-120">Fare clic su ' Avanti &gt;'.</span><span class="sxs-lookup"><span data-stu-id="65b91-120">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image4.jpg)

<span data-ttu-id="65b91-121">Dopo aver selezionato per configurare il server come Server applicazioni, verrà visualizzata questa schermata che richiede le funzionalità aggiuntive devono essere installate.</span><span class="sxs-lookup"><span data-stu-id="65b91-121">After selecting to configure the server as an Application Server, this screen will be displayed prompting what additional capabilities should be installed.</span></span> <span data-ttu-id="65b91-122">Nessuna delle due opzioni sono selezionata per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="65b91-122">Neither option is selected by default.</span></span> <span data-ttu-id="65b91-123">Per abilitare automaticamente ASP.NET, è necessario selezionare ' Abilita ASP. NET'.</span><span class="sxs-lookup"><span data-stu-id="65b91-123">To enable ASP.NET automatically, you need to select 'Enable ASP.NET'.</span></span>

<span data-ttu-id="65b91-124">Fare clic su ' Avanti &gt;'.</span><span class="sxs-lookup"><span data-stu-id="65b91-124">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image5.jpg)

<span data-ttu-id="65b91-125">Questa schermata consente di visualizzare quali sono le opzioni di installazione.</span><span class="sxs-lookup"><span data-stu-id="65b91-125">This screen displays what options are to be installed.</span></span>

<span data-ttu-id="65b91-126">Fare clic su ' Avanti &gt;'.</span><span class="sxs-lookup"><span data-stu-id="65b91-126">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image6.jpg)

<span data-ttu-id="65b91-127">Le opzioni selezionate in corso l'installazione verrà visualizzato in questa schermata.</span><span class="sxs-lookup"><span data-stu-id="65b91-127">You will see this screen while the options you selected are being installed.</span></span> <span data-ttu-id="65b91-128">È normale che altra finestra di dialogo verranno visualizzate finestre come servizi in corso l'installazione.</span><span class="sxs-lookup"><span data-stu-id="65b91-128">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="65b91-129">Potrebbe inoltre essere richiesto per la posizione del CD di installazione di Windows 2003 Server.</span><span class="sxs-lookup"><span data-stu-id="65b91-129">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="65b91-130">Fare clic su ' Avanti &gt;' al termine.</span><span class="sxs-lookup"><span data-stu-id="65b91-130">Click 'Next &gt;' when complete.</span></span>

![](aspnet-and-iis6/_static/image7.jpg)

<span data-ttu-id="65b91-131">Fare clic su 'Fine' - Windows Server 2003 è ora configurato per il supporto IIS 6.0 e ASP.NET 1.1.</span><span class="sxs-lookup"><span data-stu-id="65b91-131">Click 'Finish' - the Windows Server 2003 is now configured to support IIS 6.0 and ASP.NET 1.1.</span></span>

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a><span data-ttu-id="65b91-132">L'attivazione di IIS, opzione #2 - configurare manualmente IIS e ASP.NET</span><span class="sxs-lookup"><span data-stu-id="65b91-132">Enabling IIS, option #2 - Manually configuring IIS and ASP.NET</span></span>

<span data-ttu-id="65b91-133">Se non si desidera usare la "Configurazione guidata Server' è facoltativamente possibile installare IIS 6.0 e ASP.NET 1.1 tramite 'Installazione applicazioni' dal Pannello di controllo.</span><span class="sxs-lookup"><span data-stu-id="65b91-133">If you do not wish to use the 'Configure Your Server Wizard' you can optionally install IIS 6.0 and ASP.NET 1.1 using 'Add or Remove Programs' from the Control Panel.</span></span>

<span data-ttu-id="65b91-134">Prima di tutto, aprire il pannello di controllo:</span><span class="sxs-lookup"><span data-stu-id="65b91-134">First open the Control Panel:</span></span>

![](aspnet-and-iis6/_static/image8.jpg)

<span data-ttu-id="65b91-135">Successivamente, fare clic su ' Aggiungi/Rimuovi Windows Components' che verrà aperto 'Windows Aggiunta guidata componenti di':</span><span class="sxs-lookup"><span data-stu-id="65b91-135">Next, click on 'Add/Remove Windows Components' which will open the 'Windows Components Wizard':</span></span>

![](aspnet-and-iis6/_static/image9.jpg)

<span data-ttu-id="65b91-136">Evidenziare e selezionare "Server applicazione" e quindi fare clic su 'Dettagli'?</span><span class="sxs-lookup"><span data-stu-id="65b91-136">Highlight and check 'Application Server' and then click the 'Details?'</span></span> <span data-ttu-id="65b91-137">Pulsante:</span><span class="sxs-lookup"><span data-stu-id="65b91-137">button:</span></span>

![](aspnet-and-iis6/_static/image10.jpg)

<span data-ttu-id="65b91-138">Per installare ASP.NET, selezionare ' ASP. NET'.</span><span class="sxs-lookup"><span data-stu-id="65b91-138">To install ASP.NET, check 'ASP.NET'.</span></span>

<span data-ttu-id="65b91-139">Fare clic su 'OK' per tornare all'aggiunta guidata componenti di Windows.</span><span class="sxs-lookup"><span data-stu-id="65b91-139">Click 'OK' to return to the Windows Component Wizard.</span></span> <span data-ttu-id="65b91-140">Fare clic su ' Avanti &gt;' dall'Aggiunta guidata componenti di Windows per iniziare l'installazione:</span><span class="sxs-lookup"><span data-stu-id="65b91-140">Click 'Next &gt;' from the Windows Component Wizard to begin installing:</span></span>

![](aspnet-and-iis6/_static/image11.jpg)

<span data-ttu-id="65b91-141">È normale che altra finestra di dialogo verranno visualizzate finestre come servizi in corso l'installazione.</span><span class="sxs-lookup"><span data-stu-id="65b91-141">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="65b91-142">Potrebbe inoltre essere richiesto per la posizione del CD di installazione di Windows 2003 Server.</span><span class="sxs-lookup"><span data-stu-id="65b91-142">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="65b91-143">Al termine dell'installazione verrà visualizzato l'ultima schermata di aggiunta guidata componenti di Windows:</span><span class="sxs-lookup"><span data-stu-id="65b91-143">When installation is complete you will see the last screen of the Windows Component Wizard:</span></span>

![](aspnet-and-iis6/_static/image12.jpg)

<span data-ttu-id="65b91-144">IIS 6.0 e ASP.NET 1.1 sono ora configurati e disponibili.</span><span class="sxs-lookup"><span data-stu-id="65b91-144">IIS 6.0 and ASP.NET 1.1 are now configured and available.</span></span>

## <a name="recommended-settings"></a><span data-ttu-id="65b91-145">Impostazioni consigliate</span><span class="sxs-lookup"><span data-stu-id="65b91-145">Recommended Settings</span></span>

<span data-ttu-id="65b91-146">Durante l'esecuzione di ASP.NET 1.1 con IIS 6.0 sono disponibili diverse impostazioni di configurazione consigliati per ottenere prestazioni ottimali da ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="65b91-146">When running ASP.NET 1.1 with IIS 6.0 there are several configuration settings that are recommended to get the optimal performance from ASP.NET:</span></span>

- <span data-ttu-id="65b91-147">Configurazione dei limiti di memoria processo di lavoro</span><span class="sxs-lookup"><span data-stu-id="65b91-147">Configuring worker process memory limits</span></span>
- <span data-ttu-id="65b91-148">Configurazione riciclo dei processi di lavoro</span><span class="sxs-lookup"><span data-stu-id="65b91-148">Configuring worker process recycling</span></span>

### <a name="configuring-worker-process-memory-limits"></a><span data-ttu-id="65b91-149">Configurazione dei limiti di memoria processo di lavoro</span><span class="sxs-lookup"><span data-stu-id="65b91-149">Configuring worker process memory limits</span></span>

<span data-ttu-id="65b91-150">Per impostazione predefinita, IIS 6.0 non impostare un limite sulla quantità di memoria che è possibile utilizzare IIS.</span><span class="sxs-lookup"><span data-stu-id="65b91-150">By default IIS 6.0 does not set a limit on the amount of memory that IIS is allowed to use.</span></span> <span data-ttu-id="65b91-151">ASP. Funzionalità della Cache della rete si basa su un limite di memoria in modo che la Cache può rimuovere in modo proattivo gli elementi inusati dalla memoria.</span><span class="sxs-lookup"><span data-stu-id="65b91-151">ASP.NET's Cache feature relies on a limitation of memory so the Cache can proactively remove unused items from memory.</span></span>

<span data-ttu-id="65b91-152">È consigliabile configurare la memoria il riciclo delle funzionalità di IIS 6.0.</span><span class="sxs-lookup"><span data-stu-id="65b91-152">It is recommended that you configure the memory recycling feature of IIS 6.0.</span></span> <span data-ttu-id="65b91-153">Per configurare questo aprire Gestione Internet Information Services (Start | I programmi | Strumenti di amministrazione | Internet Information Services).</span><span class="sxs-lookup"><span data-stu-id="65b91-153">To configure this open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="65b91-154">Una volta aperto, espandere la cartella 'Pool di applicazioni':</span><span class="sxs-lookup"><span data-stu-id="65b91-154">Once open, expand the 'Application Pools' folder:</span></span>

<span data-ttu-id="65b91-155">Per ogni pool di applicazioni:</span><span class="sxs-lookup"><span data-stu-id="65b91-155">For each application pool:</span></span>

![](aspnet-and-iis6/_static/image13.jpg)

1. <span data-ttu-id="65b91-156">Fare clic sul pool di applicazioni, ad esempio "DefaultAppPool" e selezionare "proprietà":</span><span class="sxs-lookup"><span data-stu-id="65b91-156">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image14.jpg)

2. <span data-ttu-id="65b91-157">Successivamente, abilitare il riciclo memoria facendo clic su uno ' quantità massima di memoria utilizzata (in megabyte):'.</span><span class="sxs-lookup"><span data-stu-id="65b91-157">Next, enable Memory recycling by clicking on either 'Maximum used memory (in megabytes):'.</span></span> <span data-ttu-id="65b91-158">Il valore non deve essere superiore rispetto alla quantità di memoria fisica (non virtuale) nel server, una buona approssimazione è 60% della memoria fisica, ad esempio per un server con 512MB di memoria fisica, selezionare 310.</span><span class="sxs-lookup"><span data-stu-id="65b91-158">The value should not be more than the amount of physical (not virtual) memory on the server, a good approximation is 60% of the physical memory, i.e. for a server with 512MB of physical memory select 310.</span></span> <span data-ttu-id="65b91-159">È inoltre consigliabile che il valore massimo non superino 800MB quando si usa uno spazio degli indirizzi di 2GB.</span><span class="sxs-lookup"><span data-stu-id="65b91-159">It is also recommended that the maximum not exceed 800MB when using a 2GB address space.</span></span> <span data-ttu-id="65b91-160">Se lo spazio degli indirizzi di memoria del server è 3GB, il limite di memoria massima per il processo di lavoro può essere fino alto 1, 800MB:</span><span class="sxs-lookup"><span data-stu-id="65b91-160">If the memory address space of the server is 3GB, the maximum memory limit for the worker process can be as high as 1,800MB:</span></span>

![](aspnet-and-iis6/_static/image15.jpg)

<span data-ttu-id="65b91-161">Fare clic su 'Applica' e 'OK' per uscire dalla finestra di dialogo proprietà.</span><span class="sxs-lookup"><span data-stu-id="65b91-161">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="65b91-162">Ripetere questo passaggio per tutti i pool di applicazioni disponibili.</span><span class="sxs-lookup"><span data-stu-id="65b91-162">Repeat this for all available application pools.</span></span>

### <a name="configuring-worker-recycling"></a><span data-ttu-id="65b91-163">Configurazione riciclo del ruolo di lavoro</span><span class="sxs-lookup"><span data-stu-id="65b91-163">Configuring worker recycling</span></span>

<span data-ttu-id="65b91-164">Per impostazione predefinita IIS 6.0 è configurato per riciclare il processo di lavoro ogni 29 ore.</span><span class="sxs-lookup"><span data-stu-id="65b91-164">By default IIS 6.0 is configured to recycle its worker process every 29 hours.</span></span> <span data-ttu-id="65b91-165">Questo è un po' aggressivo per un'applicazione in esecuzione ASP.NET ed è consigliabile che il riciclo dei processi di lavoro automatico è disabilitato.</span><span class="sxs-lookup"><span data-stu-id="65b91-165">This is a bit aggressive for an application running ASP.NET and it is recommended that automatic worker process recycling is disabled.</span></span>

<span data-ttu-id="65b91-166">Per disabilitare il riciclo dei processi di lavoro automatico, aprire innanzitutto Gestione Internet Information Services (Start | I programmi | Strumenti di amministrazione | Internet Information Services).</span><span class="sxs-lookup"><span data-stu-id="65b91-166">To disable automatic worker process recycling, first open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="65b91-167">Una volta aperto, espandere la cartella 'Pool di applicazioni':</span><span class="sxs-lookup"><span data-stu-id="65b91-167">Once open, expand the 'Application Pools' folder:</span></span>

![](aspnet-and-iis6/_static/image16.jpg)

<span data-ttu-id="65b91-168">Per ogni pool di applicazioni:</span><span class="sxs-lookup"><span data-stu-id="65b91-168">For each application pool:</span></span>

1. <span data-ttu-id="65b91-169">Fare clic sul pool di applicazioni, ad esempio "DefaultAppPool" e selezionare "proprietà":</span><span class="sxs-lookup"><span data-stu-id="65b91-169">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image17.jpg)

2. <span data-ttu-id="65b91-170">Deselezionare l'opzione ' Ocesso di lavoro (in minuti):':</span><span class="sxs-lookup"><span data-stu-id="65b91-170">Uncheck 'Recycle worker process (in minutes):':</span></span>

![](aspnet-and-iis6/_static/image18.jpg)

<span data-ttu-id="65b91-171">Fare clic su 'Applica' e 'OK' per uscire dalla finestra di dialogo proprietà.</span><span class="sxs-lookup"><span data-stu-id="65b91-171">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="65b91-172">Ripetere questo passaggio per tutti i pool di applicazioni disponibili.</span><span class="sxs-lookup"><span data-stu-id="65b91-172">Repeat this for all available application pools.</span></span>

## <a name="granting-write-access-to-the-file-system"></a><span data-ttu-id="65b91-173">Concessione dell'accesso in scrittura nel file System</span><span class="sxs-lookup"><span data-stu-id="65b91-173">Granting write access to the file system</span></span>

<span data-ttu-id="65b91-174">Se l'applicazione richiede l'accesso in scrittura nel file System e si utilizza è necessario modificare l'elenco di controllo un accesso (ACL) per il file o cartella per concedere l'accesso ASP.NET per NTFS.</span><span class="sxs-lookup"><span data-stu-id="65b91-174">If your application requires write access to the file system and you are using NTFS you will need to modify an Access Control List (ACL) on the folder or file to grant ASP.NET access to.</span></span>

<span data-ttu-id="65b91-175">Ad esempio, per concedere ASP.NET accesso in scrittura per il c:\inetpub\wwwroot innanzitutto aprire Esplora e passare alla directory:</span><span class="sxs-lookup"><span data-stu-id="65b91-175">For example, to grant ASP.NET write access to the c:\inetpub\wwwroot first open explorer and navigate to the directory:</span></span>

![](aspnet-and-iis6/_static/image19.jpg)

<span data-ttu-id="65b91-176">Successivamente, fare doppio clic sulla directory, ad esempio 'wwwroot' e scegliere Proprietà.</span><span class="sxs-lookup"><span data-stu-id="65b91-176">Next, right-click on the directory, e.g. 'wwwroot' and select properties.</span></span> <span data-ttu-id="65b91-177">Quando si apre la finestra di dialogo proprietà, selezionare la scheda 'Sicurezza':</span><span class="sxs-lookup"><span data-stu-id="65b91-177">After the properties dialog opens, select the 'Security' tab:</span></span>

![](aspnet-and-iis6/_static/image20.jpg)

<span data-ttu-id="65b91-178">La directory c:\inetpub\wwwroot\ è una directory speciale in cui il gruppo speciale di IIS 6.0 ' IIS\_WPG' è già stata concessa lettura &amp; le autorizzazioni di esecuzione, visualizzazione contenuto cartella e lettura.</span><span class="sxs-lookup"><span data-stu-id="65b91-178">The c:\inetpub\wwwroot\ directory is a special directory in that the special IIS 6.0 group 'IIS\_WPG' is already granted Read &amp; Execute, List Folder Contents, and Read permissions.</span></span> <span data-ttu-id="65b91-179">Tuttavia, per concedere l'autorizzazione di scrittura, è necessario selezionare la casella di controllo Consenti per la scrittura:</span><span class="sxs-lookup"><span data-stu-id="65b91-179">However, to grant Write permission, you need to click the Allow checkbox for Write:</span></span>

![](aspnet-and-iis6/_static/image21.jpg)

<span data-ttu-id="65b91-180">IIS 6.0 è ora l'autorizzazione di scrittura per questa cartella.</span><span class="sxs-lookup"><span data-stu-id="65b91-180">IIS 6.0 now has write permission on this folder.</span></span> <span data-ttu-id="65b91-181">Per concedere le autorizzazioni di scrittura in altre cartelle, seguire questa procedura: tenere presente, potrebbe essere necessario aggiungere IIS\_gruppo WPG se non esiste già.</span><span class="sxs-lookup"><span data-stu-id="65b91-181">To grant write permissions on other folders, follow these steps - note, you may need to add the IIS\_WPG group if it does not already exist.</span></span>

> [!CAUTION]
> <span data-ttu-id="65b91-182">Concessione dell'autorizzazione di scrittura per IIS\_WPG consentiranno di scrivere nella directory qualsiasi applicazione ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="65b91-182">Granting write permission to IIS\_WPG will allow any ASP.NET application to write to this directory.</span></span>

## <a name="supporting-integrated-authentication-with-sql-server"></a><span data-ttu-id="65b91-183">Che supportano l'autenticazione integrata con SQL Server</span><span class="sxs-lookup"><span data-stu-id="65b91-183">Supporting integrated authentication with SQL Server</span></span>

<span data-ttu-id="65b91-184">L'autenticazione integrata consente per SQL Server sfruttare l'autenticazione di Windows NT per convalidare gli account di accesso di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="65b91-184">Integrated authentication allows for SQL Server to leverage Windows NT authentication to validate SQL Server logon accounts.</span></span> <span data-ttu-id="65b91-185">Ciò consente all'utente di ignorare il processo di accesso di SQL Server standard.</span><span class="sxs-lookup"><span data-stu-id="65b91-185">This allows the user to bypass the standard SQL Server logon process.</span></span> <span data-ttu-id="65b91-186">Con questo approccio, un utente di rete possa accedere a un database di SQL Server senza specificare una password o un ID di accesso separato in quanto SQL Server Ottiene le informazioni sull'utente e password dal processo di sicurezza di rete di Windows NT.</span><span class="sxs-lookup"><span data-stu-id="65b91-186">With this approach, a network user can access a SQL Server database without supplying a separate logon identification or password because SQL Server obtains the user and password information from the Windows NT network security process.</span></span>

<span data-ttu-id="65b91-187">Scelta dell'autenticazione integrata per le applicazioni ASP.NET è una buona scelta perché le credenziali non vengono mai archiviate all'interno della stringa di connessione per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="65b91-187">Choosing integrated authentication for ASP.NET applications is a good choice because no credentials are ever stored within your connection string for your application.</span></span> <span data-ttu-id="65b91-188">Anziché la stringa di connessione utilizzata per connettersi a SQL apparirà come segue:</span><span class="sxs-lookup"><span data-stu-id="65b91-188">Rather the connection string used to connect to SQL will look as follows:</span></span>

`"server=localhost; database=Northwind;Trusted_Connection=true"`

<span data-ttu-id="65b91-189">Questa stringa di connessione a SQL Server di usare le credenziali di Windows dell'applicazione tenta di accedere a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="65b91-189">This connection string tells SQL Server to use the Windows credentials of the application attempting to access SQL Server.</span></span> <span data-ttu-id="65b91-190">Nel caso di ASP.NET/IIS 6 questo sarebbe un account in IIS\_gruppo WPG.</span><span class="sxs-lookup"><span data-stu-id="65b91-190">In the case of ASP.NET/IIS 6 this would be an account in the IIS\_WPG group.</span></span>

<span data-ttu-id="65b91-191">Per abilitare l'autenticazione integrata tra ASP.NET e SQL Server, è necessario innanzitutto verificare che SQL Server sia configurato per l'autenticazione integrata oppure l'autenticazione modalità mista: verificare con l'amministratore del database per determinare questo.</span><span class="sxs-lookup"><span data-stu-id="65b91-191">To enable integrated authentication between SQL Server and ASP.NET, you will need to first ensure that SQL Server is configured for either Integrated authentication or Mixed-Mode authentication - check with your DBA to determine this.</span></span> <span data-ttu-id="65b91-192">Se SQL Server si trova in uno di questi due modalità, è possibile usare l'autenticazione integrata.</span><span class="sxs-lookup"><span data-stu-id="65b91-192">If SQL Server is in one of these two modes, you can use integrated authentication.</span></span>

<span data-ttu-id="65b91-193">Aprire SQL Server Enterprise Manager (Start | I programmi | Microsoft SQL Server | Enterprise Manager), selezionare il server appropriato ed espandere la cartella sicurezza:</span><span class="sxs-lookup"><span data-stu-id="65b91-193">Open SQL Server Enterprise Manager (Start | Programs | Microsoft SQL Server | Enterprise Manager), select the appropriate server, and expand the Security folder:</span></span>

![](aspnet-and-iis6/_static/image22.jpg)

<span data-ttu-id="65b91-194">Se ' BUILTINT\IIS\_WPG' gruppo non è elencato, fare clic su account di accesso e selezionare 'Nuovo account di accesso':</span><span class="sxs-lookup"><span data-stu-id="65b91-194">If 'BUILTINT\IIS\_WPG' group is not listed, right-click on Logins and select 'New Login':</span></span>

![](aspnet-and-iis6/_static/image23.jpg)

<span data-ttu-id="65b91-195">Nel ' nome:' nella casella di testo immettere ' [nome Server/dominio] \IIS\_WPG' o fare clic sul pulsante dei puntini di sospensione per aprire la selezione di utenti o gruppi di Windows NT:</span><span class="sxs-lookup"><span data-stu-id="65b91-195">In the 'Name:' textbox either enter '[Server/Domain Name]\IIS\_WPG' or click on the ellipses button to open the Windows NT user/group picker:</span></span>

![](aspnet-and-iis6/_static/image24.jpg)

<span data-ttu-id="65b91-196">Seleziona IIS corrente del computer\_gruppo WPG e fare clic su 'Add' e su OK per chiudere lo strumento di selezione.</span><span class="sxs-lookup"><span data-stu-id="65b91-196">Select the current machine's IIS\_WPG group and click 'Add' and OK to close the picker.</span></span>

<span data-ttu-id="65b91-197">È quindi necessario impostare anche il database predefinito e le autorizzazioni per accedere al database.</span><span class="sxs-lookup"><span data-stu-id="65b91-197">You then need to also set the default database and the permissions to access the database.</span></span> <span data-ttu-id="65b91-198">Per impostare il database predefinito scegliere dall'elenco a discesa sia selezionato, ad esempio Northwind riportato di seguito:</span><span class="sxs-lookup"><span data-stu-id="65b91-198">To set the default database choose from the drop down list, e.g. below Northwind is selected:</span></span>

![](aspnet-and-iis6/_static/image25.jpg)

<span data-ttu-id="65b91-199">Fare quindi clic sulla scheda di accesso al Database:</span><span class="sxs-lookup"><span data-stu-id="65b91-199">Next, click on the Database Access tab:</span></span>

![](aspnet-and-iis6/_static/image26.jpg)

<span data-ttu-id="65b91-200">Fare clic sulla casella di controllo Consenti per ogni database che si vuole consentire l'accesso a.</span><span class="sxs-lookup"><span data-stu-id="65b91-200">Click on the Permit checkbox for every database that you wish to allow access to.</span></span> <span data-ttu-id="65b91-201">È necessario anche per ruoli del database, selezionare il controllo db\_proprietario garantirà l'account di accesso dispone di tutte le autorizzazioni necessarie per gestire e utilizzare il database selezionato.</span><span class="sxs-lookup"><span data-stu-id="65b91-201">You will also need to select database roles, checking db\_owner will ensure your login has all necessary permissions to manage and use the selected database.</span></span>

<span data-ttu-id="65b91-202">Fare clic su OK per chiudere la finestra di dialogo proprietà.</span><span class="sxs-lookup"><span data-stu-id="65b91-202">Click OK to exit the property dialog.</span></span> <span data-ttu-id="65b91-203">L'applicazione ASP.NET è ora configurato per supportare l'autenticazione integrata di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="65b91-203">Your ASP.NET application is now configured to support integrated SQL Server authentication.</span></span>

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a><span data-ttu-id="65b91-204">Non eseguire ASP.NET 1.0 in modalità nativa di IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="65b91-204">Don't run ASP.NET 1.0 in IIS 6.0 native mode</span></span>

<span data-ttu-id="65b91-205">ASP.NET 1.0 in IIS 6.0 è supportato solo in modalità di compatibilità di IIS 5.</span><span class="sxs-lookup"><span data-stu-id="65b91-205">ASP.NET 1.0 on IIS 6.0 is only supported in IIS 5 compatibility mode.</span></span>

<span data-ttu-id="65b91-206">Per configurare ASP.NET 1.0 per l'esecuzione in modalità compatibilità IIS 5.0, aprire Gestione servizi Internet e fare clic con il pulsante destro siti Web e selezionare le proprietà:</span><span class="sxs-lookup"><span data-stu-id="65b91-206">To configure ASP.NET 1.0 to run in IIS 5.0 compatibility mode, open Internet Services Manager and right click Web Sites and select properties:</span></span>

![](aspnet-and-iis6/_static/image27.jpg)

<span data-ttu-id="65b91-207">Passare alla scheda del servizio e controllare? Eseguire il servizio WWW in modalità isolamento IIS 5.0?:</span><span class="sxs-lookup"><span data-stu-id="65b91-207">Switch to the Service Tab and check ?Run WWW Service in IIS 5.0 Isolation Mode?:</span></span>

![](aspnet-and-iis6/_static/image28.jpg)
