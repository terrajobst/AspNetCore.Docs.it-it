---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: La creazione di un progetto Team in TFS | Microsoft Docs
author: jrjlee
description: Questo argomento descrive come creare un nuovo progetto team in Team Foundation Server (TFS) 2010.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: 9218a22ff221dc7067662c58ccd3e758fca493b7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828313"
---
<a name="creating-a-team-project-in-tfs"></a><span data-ttu-id="aa623-103">La creazione di un progetto Team in TFS</span><span class="sxs-lookup"><span data-stu-id="aa623-103">Creating a Team Project in TFS</span></span>
====================
<span data-ttu-id="aa623-104">da [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="aa623-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="aa623-105">Scaricare PDF</span><span class="sxs-lookup"><span data-stu-id="aa623-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="aa623-106">Questo argomento descrive come creare un nuovo progetto team in Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="aa623-106">This topic describes how to create a new team project in Team Foundation Server (TFS) 2010.</span></span>


<span data-ttu-id="aa623-107">In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione aziendale di una società fittizia, denominata Fabrikam, Inc. Questa serie di esercitazioni Usa una soluzione di esempio&#x2014;il [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, una comunicazione Windows realistico Servizio Foundation (WCF) e un progetto di database.</span><span class="sxs-lookup"><span data-stu-id="aa623-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="aa623-108">Panoramica di Task</span><span class="sxs-lookup"><span data-stu-id="aa623-108">Task Overview</span></span>

<span data-ttu-id="aa623-109">Per eseguire il provisioning e usare un nuovo progetto team in TFS, è necessario completare i passaggi generali:</span><span class="sxs-lookup"><span data-stu-id="aa623-109">To provision and use a new team project in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="aa623-110">Concedere le autorizzazioni all'utente che verrà creato il nuovo progetto team.</span><span class="sxs-lookup"><span data-stu-id="aa623-110">Grant permissions to the user who will create the new team project.</span></span>
- <span data-ttu-id="aa623-111">Creare il progetto team.</span><span class="sxs-lookup"><span data-stu-id="aa623-111">Create the team project.</span></span>
- <span data-ttu-id="aa623-112">Concedere autorizzazioni ai membri del team lavorano sul progetto.</span><span class="sxs-lookup"><span data-stu-id="aa623-112">Grant permissions to the team members who will work on the project.</span></span>
- <span data-ttu-id="aa623-113">Archiviare alcuni contenuti.</span><span class="sxs-lookup"><span data-stu-id="aa623-113">Check in some content.</span></span>

<span data-ttu-id="aa623-114">In questo argomento illustrerà come eseguire queste procedure, e identificherà gli utenti e ruoli di lavoro che potrebbero essere responsabile di ogni procedura.</span><span class="sxs-lookup"><span data-stu-id="aa623-114">This topic will show you how to perform these procedures, and it will identify the users and job roles that are likely to be responsible for each procedure.</span></span> <span data-ttu-id="aa623-115">Tenere presente che, a seconda della struttura dell'organizzazione, ognuna di queste attività potrà essere la responsabilità di una persona diversa.</span><span class="sxs-lookup"><span data-stu-id="aa623-115">Be aware that, depending on the structure of your organization, each of these tasks may be the responsibility of a different person.</span></span>

<span data-ttu-id="aa623-116">Le attività e procedure dettagliate in questo argomento si presuppongono di aver installato e configurato TFS e di aver creato una raccolta di progetti team come parte del processo di configurazione.</span><span class="sxs-lookup"><span data-stu-id="aa623-116">The tasks and walkthroughs in this topic assume that you've installed and configured TFS, and that you've created a team project collection as part of the configuration process.</span></span> <span data-ttu-id="aa623-117">Per altre informazioni su queste ipotesi e per le informazioni più generali sullo scenario, vedere [configurare un Server di compilazione TFS per la distribuzione Web](configuring-a-tfs-build-server-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="aa623-117">For more information on these assumptions, and for more general background information on the scenario, see [Configure a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md).</span></span>

## <a name="grant-permissions-to-the-team-project-creator"></a><span data-ttu-id="aa623-118">Concedere le autorizzazioni per l'autore del progetto Team</span><span class="sxs-lookup"><span data-stu-id="aa623-118">Grant Permissions to the Team Project Creator</span></span>

<span data-ttu-id="aa623-119">Per creare un nuovo progetto team, è necessario queste autorizzazioni:</span><span class="sxs-lookup"><span data-stu-id="aa623-119">In order to create a new team project, you need these permissions:</span></span>

- <span data-ttu-id="aa623-120">È necessario disporre di **crea nuovi progetti** l'autorizzazione per il livello di applicazione di TFS.</span><span class="sxs-lookup"><span data-stu-id="aa623-120">You must have the **Create new projects** permission on the TFS application tier.</span></span> <span data-ttu-id="aa623-121">Questa autorizzazione viene concessa in genere mediante l'aggiunta di utenti per il **Project Collection Administrators** gruppo TFS.</span><span class="sxs-lookup"><span data-stu-id="aa623-121">You typically grant this permission by adding users to the **Project Collection Administrators** TFS group.</span></span> <span data-ttu-id="aa623-122">Il **Team Foundation Administrators** globale gruppo include anche l'autorizzazione.</span><span class="sxs-lookup"><span data-stu-id="aa623-122">The **Team Foundation Administrators** global group also includes this permission.</span></span>
- <span data-ttu-id="aa623-123">È necessario disporre dell'autorizzazione per creare nuovi siti del team della raccolta siti di SharePoint che corrisponde alla raccolta di progetti team TFS.</span><span class="sxs-lookup"><span data-stu-id="aa623-123">You must have permission to create new team sites within the SharePoint site collection that corresponds to the TFS team project collection.</span></span> <span data-ttu-id="aa623-124">È in genere concedere questa autorizzazione l'utente viene aggiunto a un gruppo di SharePoint con **controllo completo** raccolta siti di diritti in SharePoint.</span><span class="sxs-lookup"><span data-stu-id="aa623-124">You typically grant this permission by adding the user to a SharePoint group with **Full Control** rights on the SharePoint site collection.</span></span>
- <span data-ttu-id="aa623-125">Se si usano le funzionalità di SQL Server Reporting Services, è necessario essere un membro del **Gestione contenuto Team Foundation** ruolo in Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="aa623-125">If you're using SQL Server Reporting Services features, you must be a member of the **Team Foundation Content Manager** role in Reporting Services.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="aa623-126">Che consente di eseguire queste procedure?</span><span class="sxs-lookup"><span data-stu-id="aa623-126">Who Performs These Procedures?</span></span>

<span data-ttu-id="aa623-127">In genere, la persona o gruppo che amministra nella distribuzione TFS esegue anche queste procedure.</span><span class="sxs-lookup"><span data-stu-id="aa623-127">Typically, the person or group who administers the TFS deployment also performs these procedures.</span></span>

<span data-ttu-id="aa623-128">Poiché si tratta di un set di autorizzazioni con privilegiato elevati, i nuovi progetti team vengono in genere creati da un piccolo subset di utenti la responsabilità di amministrare una distribuzione TFS.</span><span class="sxs-lookup"><span data-stu-id="aa623-128">Because this is a highly privileged set of permissions, new team projects are typically created by a small subset of users with responsibility for administering a TFS deployment.</span></span> <span data-ttu-id="aa623-129">Gli sviluppatori saranno non in genere essere concesse le autorizzazioni necessarie per creare nuovi progetti team.</span><span class="sxs-lookup"><span data-stu-id="aa623-129">Developers will not usually be granted the permissions required to create new team projects.</span></span>

### <a name="grant-permissions-in-tfs"></a><span data-ttu-id="aa623-130">Concedere le autorizzazioni in TFS</span><span class="sxs-lookup"><span data-stu-id="aa623-130">Grant Permissions in TFS</span></span>

<span data-ttu-id="aa623-131">Se si desidera consentire agli utenti di creare nuovi progetti team, la prima attività di alto livello consiste nell'aggiungere l'utente per il **Project Collection Administrators** gruppo per la raccolta di progetti team.</span><span class="sxs-lookup"><span data-stu-id="aa623-131">If you want to enable a user to create new team projects, the first high-level task is to add the user to the **Project Collection Administrators** group for the team project collection.</span></span>

<span data-ttu-id="aa623-132">**Per aggiungere un utente al gruppo Project Collection Administrators**</span><span class="sxs-lookup"><span data-stu-id="aa623-132">**To add a user to the Project Collection Administrators group**</span></span>

1. <span data-ttu-id="aa623-133">Nel server TFS, nel **avviare** dal menu **tutti i programmi**, fare clic su **Microsoft Team Foundation Server 2010**, quindi fare clic su **Team Foundation Console di amministrazione**.</span><span class="sxs-lookup"><span data-stu-id="aa623-133">On the TFS server, on the **Start** menu, point to **All Programs**, click **Microsoft Team Foundation Server 2010**, and then click **Team Foundation Administration Console**.</span></span>
2. <span data-ttu-id="aa623-134">Nella visualizzazione ad albero di navigazione, espandere **livello di applicazione**, quindi fare clic su **raccolte di progetti Team**.</span><span class="sxs-lookup"><span data-stu-id="aa623-134">In the navigation tree view, expand **Application Tier**, and then click **Team Project Collections**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. <span data-ttu-id="aa623-135">Nel **raccolte di progetti Team** riquadro, selezionare il progetto team di raccolta che si desidera gestire.</span><span class="sxs-lookup"><span data-stu-id="aa623-135">In the **Team Project Collections** pane, select the team project collection you want to manage.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. <span data-ttu-id="aa623-136">Nel **generali** scheda, fare clic su **l'appartenenza al gruppo**.</span><span class="sxs-lookup"><span data-stu-id="aa623-136">On the **General** tab, click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. <span data-ttu-id="aa623-137">Nel **gruppi globali** finestra di dialogo, seleziona la **Project Collection Administrators** gruppo e quindi fare clic su **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="aa623-137">In the **Global Groups** dialog box, select the **Project Collection Administrators** group, and then click **Properties**.</span></span>
6. <span data-ttu-id="aa623-138">Nel **proprietà gruppo di Team Foundation Server** finestra di dialogo **Windows utente o gruppo**e quindi fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="aa623-138">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. <span data-ttu-id="aa623-139">Nel **Seleziona utenti, computer o gruppi** finestra di dialogo digitare il nome utente dell'utente che si desidera essere in grado di creare nuovi progetti team, fare clic su **Controlla nomi**, quindi fare clic su **OK** .</span><span class="sxs-lookup"><span data-stu-id="aa623-139">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to be able to create new team projects, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. <span data-ttu-id="aa623-140">Nel **proprietà gruppo di Team Foundation Server** finestra di dialogo, fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="aa623-140">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
9. <span data-ttu-id="aa623-141">Nel **gruppi globali** finestra di dialogo, fare clic su **Chiudi**.</span><span class="sxs-lookup"><span data-stu-id="aa623-141">In the **Global Groups** dialog box, click **Close**.</span></span>

### <a name="grant-permissions-in-sharepoint-services"></a><span data-ttu-id="aa623-142">Concedere le autorizzazioni di SharePoint Services</span><span class="sxs-lookup"><span data-stu-id="aa623-142">Grant Permissions in SharePoint Services</span></span>

<span data-ttu-id="aa623-143">Successivamente, è necessario concedere all'utente l'autorizzazione per creare nuovi siti del team della raccolta siti di SharePoint che corrisponde alla raccolta di progetti team TFS.</span><span class="sxs-lookup"><span data-stu-id="aa623-143">Next, you need to give the user permission to create new team sites in the SharePoint site collection that corresponds to your TFS team project collection.</span></span>

<span data-ttu-id="aa623-144">**Per concedere le autorizzazioni controllo completo per la raccolta di siti di SharePoint**</span><span class="sxs-lookup"><span data-stu-id="aa623-144">**To grant Full Control permissions on the SharePoint site collection**</span></span>

1. <span data-ttu-id="aa623-145">Nella Console di amministrazione del Team Foundation Server sul **raccolte di progetti Team** pagina, selezionare la raccolta di progetti team che si desidera gestire.</span><span class="sxs-lookup"><span data-stu-id="aa623-145">In the Team Foundation Server Administration Console, on the **Team Project Collections** page, select the team project collection you want to manage.</span></span>
2. <span data-ttu-id="aa623-146">Nel **sito di SharePoint** scheda, prendere nota del valore della **percorso corrente sito predefinito** URL.</span><span class="sxs-lookup"><span data-stu-id="aa623-146">On the **SharePoint Site** tab, note the value of the **Current Default Site Location** URL.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. <span data-ttu-id="aa623-147">Aprire Internet Explorer e quindi passare all'URL di cui si è preso nota nel passaggio 2.</span><span class="sxs-lookup"><span data-stu-id="aa623-147">Open Internet Explorer, and then go to the URL you noted in step 2.</span></span>

    > [!NOTE]
    > <span data-ttu-id="aa623-148">Se si non sta connessi a Windows dell'utente che ha creato la raccolta di progetti team, è necessario accedere a SharePoint con questo account utente per continuare.</span><span class="sxs-lookup"><span data-stu-id="aa623-148">If you're not logged on to Windows as the user who created the team project collection, you'll need to sign in to SharePoint as this user in order to continue.</span></span>
4. <span data-ttu-id="aa623-149">Nel **Azioni sito** menu, fare clic su **Impostazioni sito**.</span><span class="sxs-lookup"><span data-stu-id="aa623-149">On the **Site Actions** menu, click **Site Settings**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. <span data-ttu-id="aa623-150">Nel **Impostazioni sito** nella pagina **utenti e autorizzazioni**, fare clic su **utenti e gruppi**.</span><span class="sxs-lookup"><span data-stu-id="aa623-150">On the **Site Settings** page, under **Users and Permissions**, click **People and groups**.</span></span>
6. <span data-ttu-id="aa623-151">Nel riquadro di spostamento sinistro, fare clic su **gruppi**.</span><span class="sxs-lookup"><span data-stu-id="aa623-151">In the left navigation panel, click **Groups**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. <span data-ttu-id="aa623-152">Nel **utenti e gruppi: tutti i gruppi** pagina, fare clic su **Imposta gruppi per questo sito**.</span><span class="sxs-lookup"><span data-stu-id="aa623-152">On the **People and Groups: All Groups** page, click **Set Up Groups for this Site**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > <span data-ttu-id="aa623-153">È possibile ricevere un' <strong>HTTP 404 non trovato</strong> errore a causa di un bug di codifica HTTP doppie.</span><span class="sxs-lookup"><span data-stu-id="aa623-153">You may receive an <strong>HTTP 404 Not Found</strong> error due to a double HTTP encoding bug.</span></span> <span data-ttu-id="aa623-154">In questo caso, sostituire l'URL con questo:</span><span class="sxs-lookup"><span data-stu-id="aa623-154">If this occurs, replace the URL with this:</span></span>   
   > <span data-ttu-id="aa623-155">`[site_collection_URL]/_layouts/permsetup.aspx` Per esempio:</span><span class="sxs-lookup"><span data-stu-id="aa623-155">`[site_collection_URL]/_layouts/permsetup.aspx` For example:</span></span>  
   > `http://tfs/sites/Fabrikam%20Web%20Projects/_layouts/permsetup.aspx` 
8. <span data-ttu-id="aa623-156">Nel **Imposta gruppi per questo sito** pagina, aggiungere l'utente che crea i progetti team per il **proprietari** gruppo e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="aa623-156">On the **Set Up Groups for this Site** page, add the user who will create team projects to the **Owners** group, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image10.png)

<span data-ttu-id="aa623-157">Per altre informazioni su come abilitare gli utenti di creare nuovi progetti team all'interno di una raccolta di progetti team, vedere [impostare le autorizzazioni di amministratore per raccolte di progetti Team](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="aa623-157">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span>

## <a name="create-a-new-team-project-and-add-users"></a><span data-ttu-id="aa623-158">Creare un nuovo progetto Team e aggiungere utenti</span><span class="sxs-lookup"><span data-stu-id="aa623-158">Create a New Team Project and Add Users</span></span>

<span data-ttu-id="aa623-159">Dopo aver creato le autorizzazioni necessarie, è possibile usare la **Team Explorer** finestra in Visual Studio 2010 per creare un nuovo progetto team.</span><span class="sxs-lookup"><span data-stu-id="aa623-159">Once you have the necessary permissions, you can use the **Team Explorer** window in Visual Studio 2010 to create a new team project.</span></span> <span data-ttu-id="aa623-160">Questo approccio offre una procedura guidata che consente di raccogliere tutte le informazioni necessarie ed esegue le attività necessarie in TFS, SharePoint e SQL Server Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="aa623-160">This approach provides a wizard that collects all the required information and performs the necessary tasks in TFS, SharePoint, and SQL Server Reporting Services.</span></span> <span data-ttu-id="aa623-161">È inoltre necessario concedere le autorizzazioni per il nuovo progetto team per i membri del team di sviluppo, per consentire loro di aggiungere e modificare il contenuto.</span><span class="sxs-lookup"><span data-stu-id="aa623-161">You'll also need to grant permissions on the new team project to members of the developer team, to enable them to add and modify content.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="aa623-162">Che consente di eseguire queste procedure?</span><span class="sxs-lookup"><span data-stu-id="aa623-162">Who Performs These Procedures?</span></span>

<span data-ttu-id="aa623-163">In genere un amministratore di TFS o il responsabile del team di sviluppo consente di eseguire queste procedure.</span><span class="sxs-lookup"><span data-stu-id="aa623-163">Usually either a TFS administrator or a developer team leader performs these procedures.</span></span>

### <a name="create-a-new-team-project"></a><span data-ttu-id="aa623-164">Creare un nuovo progetto Team</span><span class="sxs-lookup"><span data-stu-id="aa623-164">Create a New Team Project</span></span>

<span data-ttu-id="aa623-165">La procedura seguente descrive come creare un nuovo progetto team in TFS 2010.</span><span class="sxs-lookup"><span data-stu-id="aa623-165">The next procedure describes how to create a new team project in TFS 2010.</span></span>

<span data-ttu-id="aa623-166">**Per creare un nuovo progetto team**</span><span class="sxs-lookup"><span data-stu-id="aa623-166">**To create a new team project**</span></span>

1. <span data-ttu-id="aa623-167">Nel **avviare** dal menu **tutti i programmi**, fare clic su **Microsoft Visual Studio 2010**, fare doppio clic su **Microsoft Visual Studio 2010**, e quindi fare clic su **Esegui come amministratore**.</span><span class="sxs-lookup"><span data-stu-id="aa623-167">On the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, right-click **Microsoft Visual Studio 2010**, and then click **Run as administrator**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="aa623-168">Se non si esegue Visual Studio 2010 come amministratore, la creazione guidata nuovo progetto Team avrà esito negativo l'ultimo passaggio.</span><span class="sxs-lookup"><span data-stu-id="aa623-168">If you don't run Visual Studio 2010 as an administrator, the New Team Project Wizard will fail on the last step.</span></span>
2. <span data-ttu-id="aa623-169">Se il **User Account Control** verrà visualizzata la finestra di dialogo, fare clic su **Yes**.</span><span class="sxs-lookup"><span data-stu-id="aa623-169">If the **User Account Control** dialog box appears, click **Yes**.</span></span>
3. <span data-ttu-id="aa623-170">In Visual Studio sul **Team** menu, fare clic su **Connetti a Team Foundation Server**.</span><span class="sxs-lookup"><span data-stu-id="aa623-170">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="aa623-171">Se già stata configurata una connessione a un server TFS, è possibile omettere i passaggi da 4 a 7.</span><span class="sxs-lookup"><span data-stu-id="aa623-171">If you have already configured a connection to a TFS server, you can omit steps 4-7.</span></span>
4. <span data-ttu-id="aa623-172">Nel **connessione al progetto Team** finestra di dialogo, fare clic su **server**.</span><span class="sxs-lookup"><span data-stu-id="aa623-172">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
5. <span data-ttu-id="aa623-173">Nel **Aggiungi/Rimuovi Team Foundation Server** finestra di dialogo, fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="aa623-173">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
6. <span data-ttu-id="aa623-174">Nel **Aggiungi Team Foundation Server** finestra di dialogo, fornire i dettagli dell'istanza di TFS e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="aa623-174">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. <span data-ttu-id="aa623-175">Nel **Aggiungi/Rimuovi Team Foundation Server** finestra di dialogo, fare clic su **Chiudi**.</span><span class="sxs-lookup"><span data-stu-id="aa623-175">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
8. <span data-ttu-id="aa623-176">Nel **Connetti a progetto Team** finestra di dialogo, seleziona l'istanza di TFS si desidera connettersi, per selezionare il team di progetto insieme si desidera aggiungere a e quindi fare clic su **Connect**.</span><span class="sxs-lookup"><span data-stu-id="aa623-176">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection you want to add to, and then click **Connect**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. <span data-ttu-id="aa623-177">Nel **Team Explorer** finestra, pulsante destro del mouse nella raccolta di progetti team e quindi fare clic su **nuovo progetto Team**.</span><span class="sxs-lookup"><span data-stu-id="aa623-177">In the **Team Explorer** window, right-click the team project collection, and then click **New Team Project**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. <span data-ttu-id="aa623-178">Nel **nuovo progetto Team** della finestra di dialogo specificare un nome e una descrizione per il progetto team e quindi fare clic su **successivo**.</span><span class="sxs-lookup"><span data-stu-id="aa623-178">In the **New Team Project** dialog box, provide a name and a description for the team project, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="aa623-179">Se il progetto team include spazi, si potrebbero riscontrare alcuni problemi quando si accede a usare lo strumento di distribuzione di Internet Information Services (IIS) Web (distribuzione Web) per distribuire pacchetti nel percorso di output.</span><span class="sxs-lookup"><span data-stu-id="aa623-179">If your team project includes spaces, you may face some issues when you come to use the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to deploy packages from the output path.</span></span> <span data-ttu-id="aa623-180">Spazi nel percorso possono rendere molto più difficile eseguire i comandi di distribuzione Web.</span><span class="sxs-lookup"><span data-stu-id="aa623-180">Spaces in the path can make it a lot more difficult to run Web Deploy commands.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. <span data-ttu-id="aa623-181">Nel **selezionare un modello di processo** pagina, selezionare il modello di processo che si desidera usare per gestire il processo di sviluppo e quindi fare clic su **successivo**.</span><span class="sxs-lookup"><span data-stu-id="aa623-181">On the **Select a Process Template** page, select the process template that you want to use to manage the development process, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="aa623-182">Per altre informazioni sui modelli di processo per TFS, vedere [modelli di processo e gli strumenti](https://msdn.microsoft.com/vstudio/aa718795).</span><span class="sxs-lookup"><span data-stu-id="aa623-182">For more information on process templates for TFS, see [Process Templates and Tools](https://msdn.microsoft.com/vstudio/aa718795).</span></span>
12. <span data-ttu-id="aa623-183">Nel **Impostazioni sito Team** pagina, lasciare invariate le impostazioni predefinite e quindi fare clic su **successivo**.</span><span class="sxs-lookup"><span data-stu-id="aa623-183">On the **Team Site Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
13. <span data-ttu-id="aa623-184">Questa impostazione crea o identifica, un sito del team SharePoint che viene associato al progetto team TFS.</span><span class="sxs-lookup"><span data-stu-id="aa623-184">This setting creates, or identifies, a SharePoint team site that is associated with the TFS team project.</span></span> <span data-ttu-id="aa623-185">Il team di sviluppo è possibile utilizzare questo sito per gestire la documentazione, partecipare al thread di discussione, creare pagine wiki e varie altre attività che non sono correlati al codice.</span><span class="sxs-lookup"><span data-stu-id="aa623-185">Your development team can use this site to manage documentation, participate in discussion threads, create wiki pages, and perform various other tasks that are not related to code.</span></span> <span data-ttu-id="aa623-186">Per altre informazioni, vedere [interazioni tra i prodotti SharePoint e Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span><span class="sxs-lookup"><span data-stu-id="aa623-186">For more information, see [Interactions Between SharePoint Products and Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span></span>
14. <span data-ttu-id="aa623-187">Nel **specificare le impostazioni di codice sorgente** pagina, lasciare invariate le impostazioni predefinite e quindi fare clic su **successivo**.</span><span class="sxs-lookup"><span data-stu-id="aa623-187">On the **Specify Source Control Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
15. <span data-ttu-id="aa623-188">Questa impostazione identifica o crea la posizione nella gerarchia delle cartelle TFS che fungerà da una cartella radice per il contenuto.</span><span class="sxs-lookup"><span data-stu-id="aa623-188">This setting identifies or creates the location in the TFS folder hierarchy that will act as a root folder for your content.</span></span>
16. <span data-ttu-id="aa623-189">Nel **confermare le impostazioni del progetto Team** pagina, fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="aa623-189">On the **Confirm Team Project Settings** page, click **Finish**.</span></span>
17. <span data-ttu-id="aa623-190">Quando il nuovo progetto team creato, scegliere il **progetto Team creato** pagina, fare clic su **Chiudi**.</span><span class="sxs-lookup"><span data-stu-id="aa623-190">When the new team project is successfully created, on the **Team Project Created** page, click **Close**.</span></span>

### <a name="add-users-to-a-team-project"></a><span data-ttu-id="aa623-191">Aggiungere utenti a un progetto Team</span><span class="sxs-lookup"><span data-stu-id="aa623-191">Add Users to a Team Project</span></span>

<span data-ttu-id="aa623-192">Ora che è stato creato il nuovo progetto team, è possibile concedere autorizzazioni agli utenti per consentire loro di avviare l'aggiunta e la collaborazione sul contenuto.</span><span class="sxs-lookup"><span data-stu-id="aa623-192">Now that you've created the new team project, you can grant permissions to users to enable them to start adding and collaborating on content.</span></span>

<span data-ttu-id="aa623-193">**Per aggiungere utenti a un progetto team**</span><span class="sxs-lookup"><span data-stu-id="aa623-193">**To add users to a team project**</span></span>

1. <span data-ttu-id="aa623-194">In Visual Studio 2010, nelle **Team Explorer** finestra, fare clic sul progetto team, scegliere **impostazioni progetto Team**, quindi fare clic su **l'appartenenza al gruppo**.</span><span class="sxs-lookup"><span data-stu-id="aa623-194">In Visual Studio 2010, in the **Team Explorer** window, right-click the team project, point to **Team Project Settings**, and then click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. <span data-ttu-id="aa623-195">Per consentire agli utenti di aggiungere, modificare e rimuovere codice sottoposto a controllo del codice sorgente, è necessario aggiungere all'utente per il **collaboratori** gruppo.</span><span class="sxs-lookup"><span data-stu-id="aa623-195">To enable a user to add, modify, and remove code under source control, you need to add him or her to the **Contributors** group.</span></span>
3. <span data-ttu-id="aa623-196">Nel **gruppi di progetto** finestra di dialogo, seleziona la **collaboratori** gruppo e quindi fare clic su **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="aa623-196">In the **Project Groups** dialog box, select the **Contributors** group, and then click **Properties**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. <span data-ttu-id="aa623-197">Nel **proprietà gruppo di Team Foundation Server** finestra di dialogo **Windows utente o gruppo**e quindi fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="aa623-197">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. <span data-ttu-id="aa623-198">Nel **Seleziona utenti, computer o gruppi** finestra di dialogo digitare il nome utente dell'utente da aggiungere al progetto team, fare clic su **Controlla nomi**, quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="aa623-198">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to add to the team project, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. <span data-ttu-id="aa623-199">Nel **proprietà gruppo di Team Foundation Server** finestra di dialogo, fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="aa623-199">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
7. <span data-ttu-id="aa623-200">Nel **gruppi di progetto** finestra di dialogo, fare clic su **Chiudi**.</span><span class="sxs-lookup"><span data-stu-id="aa623-200">In the **Project Groups** dialog box, click **Close**.</span></span>

## <a name="conclusion"></a><span data-ttu-id="aa623-201">Conclusione</span><span class="sxs-lookup"><span data-stu-id="aa623-201">Conclusion</span></span>

<span data-ttu-id="aa623-202">A questo punto, il nuovo progetto team è pronto per l'uso e al team di sviluppatori può avviare l'aggiunta di contenuto e la collaborazione al processo di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="aa623-202">At this point, your new team project is ready to use, and your developer team can start adding content and collaborating on the development process.</span></span>

<span data-ttu-id="aa623-203">Argomento successivo [aggiunta di contenuto a controllo del codice sorgente](adding-content-to-source-control.md), viene descritto come aggiungere contenuto al controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="aa623-203">The next topic, [Adding Content to Source Control](adding-content-to-source-control.md), describes how to add content to source control.</span></span>

## <a name="further-reading"></a><span data-ttu-id="aa623-204">Ulteriori informazioni</span><span class="sxs-lookup"><span data-stu-id="aa623-204">Further Reading</span></span>

<span data-ttu-id="aa623-205">Per più ampio indicazioni sulla creazione di progetti team in TFS, vedere [creare un progetto Team](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="aa623-205">For broader guidance on creating team projects in TFS, see [Create a Team Project](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span></span> <span data-ttu-id="aa623-206">Per altre informazioni su come abilitare gli utenti di creare nuovi progetti team all'interno di una raccolta di progetti team, vedere [impostare le autorizzazioni di amministratore per raccolte di progetti Team](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="aa623-206">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span> <span data-ttu-id="aa623-207">Per altre informazioni su come aggiungere utenti ai progetti team, vedere [aggiungere utenti ai progetti Team](https://msdn.microsoft.com/library/bb558971.aspx).</span><span class="sxs-lookup"><span data-stu-id="aa623-207">For more information on adding users to team projects, see [Add Users to Team Projects](https://msdn.microsoft.com/library/bb558971.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="aa623-208">[Precedente](configuring-team-foundation-server-for-web-deployment.md)
> [Successivo](adding-content-to-source-control.md)</span><span class="sxs-lookup"><span data-stu-id="aa623-208">[Previous](configuring-team-foundation-server-for-web-deployment.md)
[Next](adding-content-to-source-control.md)</span></span>
