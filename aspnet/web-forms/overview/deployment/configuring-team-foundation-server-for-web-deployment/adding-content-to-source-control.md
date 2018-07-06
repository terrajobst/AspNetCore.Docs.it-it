---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: Aggiunta di contenuto al controllo del codice sorgente | Microsoft Docs
author: jrjlee
description: In questo argomento viene illustrato come aggiungere contenuto al controllo del codice sorgente in Team Foundation Server (TFS) 2010. Viene descritto come aggiungere soluzioni e progetti a un progetto di un team...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: 9fdb2e37f2925273b457157b634865d93e865098
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838371"
---
<a name="adding-content-to-source-control"></a><span data-ttu-id="1ed33-104">Aggiunta di contenuto a controllo del codice sorgente</span><span class="sxs-lookup"><span data-stu-id="1ed33-104">Adding Content to Source Control</span></span>
====================
<span data-ttu-id="1ed33-105">da [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="1ed33-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="1ed33-106">Scaricare PDF</span><span class="sxs-lookup"><span data-stu-id="1ed33-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="1ed33-107">In questo argomento viene illustrato come aggiungere contenuto al controllo del codice sorgente in Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="1ed33-107">This topic explains how to add content to source control in Team Foundation Server (TFS) 2010.</span></span> <span data-ttu-id="1ed33-108">Viene descritto come aggiungere soluzioni e progetti a un progetto team in TFS e viene spiegato come aggiungere le dipendenze esterne, ad esempio Framework o assembly al controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="1ed33-108">It describes how to add solutions and projects to a team project in TFS, and it explains how to add external dependencies like frameworks or assemblies to source control.</span></span>


<span data-ttu-id="1ed33-109">In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione aziendale di una società fittizia, denominata Fabrikam, Inc. Questa serie di esercitazioni Usa una soluzione di esempio&#x2014;il [soluzione Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, una comunicazione Windows realistico Servizio Foundation (WCF) e un progetto di database.</span><span class="sxs-lookup"><span data-stu-id="1ed33-109">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="1ed33-110">Panoramica di Task</span><span class="sxs-lookup"><span data-stu-id="1ed33-110">Task Overview</span></span>

<span data-ttu-id="1ed33-111">Nella maggior parte dei casi, ogni membro del team di sviluppo deve essere in grado di aggiungere contenuto al controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="1ed33-111">In most cases, every member of the developer team should be able to add content to source control.</span></span> <span data-ttu-id="1ed33-112">Per aggiungere una soluzione a controllo del codice sorgente in TFS, è necessario completare i passaggi generali:</span><span class="sxs-lookup"><span data-stu-id="1ed33-112">To add a solution to source control in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="1ed33-113">Connettersi a un progetto team.</span><span class="sxs-lookup"><span data-stu-id="1ed33-113">Connect to a team project.</span></span>
- <span data-ttu-id="1ed33-114">Mappare la struttura di cartelle del progetto team sul server a una struttura di cartelle nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="1ed33-114">Map the team project folder structure on the server to a folder structure on your local computer.</span></span>
- <span data-ttu-id="1ed33-115">Aggiungere la soluzione e il relativo contenuto al controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="1ed33-115">Add the solution and its contents to source control.</span></span>
- <span data-ttu-id="1ed33-116">Aggiungere le dipendenze esterne a controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="1ed33-116">Add any external dependencies to source control.</span></span>

<span data-ttu-id="1ed33-117">In questo argomento illustrerà come eseguire queste procedure.</span><span class="sxs-lookup"><span data-stu-id="1ed33-117">This topic will show you how to perform these procedures.</span></span>

<span data-ttu-id="1ed33-118">Le attività e procedure dettagliate in questo argomento si presuppongono di avere già creato un nuovo progetto team TFS per gestire il contenuto.</span><span class="sxs-lookup"><span data-stu-id="1ed33-118">The tasks and walkthroughs in this topic assume that you've already created a new TFS team project to manage your content.</span></span> <span data-ttu-id="1ed33-119">Per altre informazioni sulla creazione di un nuovo progetto team, vedere [la creazione di un progetto Team in TFS](creating-a-team-project-in-tfs.md).</span><span class="sxs-lookup"><span data-stu-id="1ed33-119">For more information on creating a new team project, see [Creating a Team Project in TFS](creating-a-team-project-in-tfs.md).</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="1ed33-120">Che consente di eseguire queste procedure?</span><span class="sxs-lookup"><span data-stu-id="1ed33-120">Who Performs These Procedures?</span></span>

<span data-ttu-id="1ed33-121">Nella maggior parte dei casi, ogni membro del team di sviluppo deve essere in grado di aggiungere e modificare il contenuto all'interno dei progetti team specifico.</span><span class="sxs-lookup"><span data-stu-id="1ed33-121">In most cases, every member of the developer team should be able to add and modify content within specific team projects.</span></span>

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a><span data-ttu-id="1ed33-122">Connettersi a un progetto Team e creare un Mapping della cartella</span><span class="sxs-lookup"><span data-stu-id="1ed33-122">Connect to a Team Project and Create a Folder Mapping</span></span>

<span data-ttu-id="1ed33-123">Prima di aggiungere contenuto al controllo del codice sorgente, è necessario connettersi a un progetto team e creare un mapping tra la struttura di cartelle nel server e il file system nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="1ed33-123">Before you add any content to source control, you need to connect to a team project and create a mapping between the folder structure on the server and the file system on your local machine.</span></span>

<span data-ttu-id="1ed33-124">**Per connettersi a un progetto team ed eseguire il mapping di un percorso locale**</span><span class="sxs-lookup"><span data-stu-id="1ed33-124">**To connect to a team project and map a local path**</span></span>

1. <span data-ttu-id="1ed33-125">Nella workstation per gli sviluppatori, aprire Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="1ed33-125">On your developer workstation, open Visual Studio 2010.</span></span>
2. <span data-ttu-id="1ed33-126">In Visual Studio sul **Team** menu, fare clic su **Connetti a Team Foundation Server**.</span><span class="sxs-lookup"><span data-stu-id="1ed33-126">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1ed33-127">Se già stata configurata una connessione a un server TFS, è possibile omettere i passaggi da 3 a 6.</span><span class="sxs-lookup"><span data-stu-id="1ed33-127">If you have already configured a connection to a TFS server, you can omit steps 3-6.</span></span>
3. <span data-ttu-id="1ed33-128">Nel **connessione al progetto Team** finestra di dialogo, fare clic su **server**.</span><span class="sxs-lookup"><span data-stu-id="1ed33-128">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
4. <span data-ttu-id="1ed33-129">Nel **Aggiungi/Rimuovi Team Foundation Server** finestra di dialogo, fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="1ed33-129">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
5. <span data-ttu-id="1ed33-130">Nel **Aggiungi Team Foundation Server** finestra di dialogo, fornire i dettagli dell'istanza di TFS e quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="1ed33-130">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](adding-content-to-source-control/_static/image1.png)
6. <span data-ttu-id="1ed33-131">Nel **Aggiungi/Rimuovi Team Foundation Server** finestra di dialogo, fare clic su **Chiudi**.</span><span class="sxs-lookup"><span data-stu-id="1ed33-131">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
7. <span data-ttu-id="1ed33-132">Nel **Connetti a progetto Team** finestra di dialogo, selezionare l'istanza di TFS si desidera connettersi, per selezionare il team di progetto selezionare il progetto team da aggiungere alla raccolta e quindi fare clic su **Connect**.</span><span class="sxs-lookup"><span data-stu-id="1ed33-132">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection, select the team project you want to add to, and then click **Connect**.</span></span>

    ![](adding-content-to-source-control/_static/image2.png)
8. <span data-ttu-id="1ed33-133">Nel **Team Explorer** finestra, espandere il progetto team e quindi fare doppio clic su **controllo del codice sorgente**.</span><span class="sxs-lookup"><span data-stu-id="1ed33-133">In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image3.png)
9. <span data-ttu-id="1ed33-134">Nel **Esplora controllo codice sorgente** scheda, fare clic su **non mappato**.</span><span class="sxs-lookup"><span data-stu-id="1ed33-134">On the **Source Control Explorer** tab, click **Not mapped**.</span></span>

    ![](adding-content-to-source-control/_static/image4.png)
10. <span data-ttu-id="1ed33-135">Nel **mappa** nella finestra di dialogo il **cartella locale** casella, selezionare (o creare) una cartella locale per agire come cartella radice per il progetto team e quindi fare clic su **mappa**.</span><span class="sxs-lookup"><span data-stu-id="1ed33-135">In the **Map** dialog box, in the **Local folder** box, browse to (or create) a local folder to act as the root folder for the team project, and then click **Map**.</span></span>

    ![](adding-content-to-source-control/_static/image5.png)
11. <span data-ttu-id="1ed33-136">Quando viene chiesto di scaricare i file di origine, fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="1ed33-136">When you're prompted to download source files, click **Yes**.</span></span>

    ![](adding-content-to-source-control/_static/image6.png)

<span data-ttu-id="1ed33-137">A questo punto, è stato eseguito il mapping di cartella sul lato server per il progetto team in una cartella locale nella workstation di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="1ed33-137">At this point, you have mapped the server-side folder for the team project to a local folder on your developer workstation.</span></span> <span data-ttu-id="1ed33-138">Inoltre scaricato qualsiasi contenuto esistente dal progetto team per la struttura di cartelle locali.</span><span class="sxs-lookup"><span data-stu-id="1ed33-138">You've also downloaded any existing content from the team project to your local folder structure.</span></span> <span data-ttu-id="1ed33-139">È ora possibile iniziare ad aggiungere il proprio contenuto al controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="1ed33-139">You can now start to add your own content to source control.</span></span>

## <a name="add-projects-and-solutions-to-source-control"></a><span data-ttu-id="1ed33-140">Aggiungere soluzioni e progetti al controllo del codice sorgente</span><span class="sxs-lookup"><span data-stu-id="1ed33-140">Add Projects and Solutions to Source Control</span></span>

<span data-ttu-id="1ed33-141">Per aggiungere soluzioni e progetti al controllo del codice sorgente, è innanzitutto necessario per spostarli nella cartella mappata per il progetto team nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="1ed33-141">To add projects and solutions to source control, you first need to move them to the mapped folder for the team project on your local machine.</span></span> <span data-ttu-id="1ed33-142">È quindi possibile archiviare il contenuto da sincronizzare le aggiunte con il server.</span><span class="sxs-lookup"><span data-stu-id="1ed33-142">You can then check in the content to synchronize your additions with the server.</span></span>

<span data-ttu-id="1ed33-143">**Aggiungere progetti al controllo del codice sorgente**</span><span class="sxs-lookup"><span data-stu-id="1ed33-143">**To add projects to source control**</span></span>

1. <span data-ttu-id="1ed33-144">Nella workstation per gli sviluppatori, spostare i progetti e soluzioni in una posizione appropriata all'interno della struttura di cartella mappata per il progetto team.</span><span class="sxs-lookup"><span data-stu-id="1ed33-144">On your developer workstation, move your projects and solutions to an appropriate location within the mapped folder structure for the team project.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1ed33-145">Molte organizzazioni presentano un approccio preferito per la modalità progetti e soluzioni devono essere organizzate in controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="1ed33-145">Many organizations will have a preferred approach to how projects and solutions should be organized in source control.</span></span> <span data-ttu-id="1ed33-146">Per indicazioni su come le cartelle di struttura, vedere [How To: struttura del controllo cartelle del codice sorgente in Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span><span class="sxs-lookup"><span data-stu-id="1ed33-146">For guidance on how to structure folders, see [How To: Structure Your Source Control Folders in Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span></span>
2. <span data-ttu-id="1ed33-147">Aprire la soluzione in Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="1ed33-147">Open the solution in Visual Studio 2010.</span></span>
3. <span data-ttu-id="1ed33-148">Nel **Esplora soluzioni** finestra, fare doppio clic la soluzione e quindi fare clic su **Aggiungi soluzione al controllo del codice sorgente**.</span><span class="sxs-lookup"><span data-stu-id="1ed33-148">In the **Solution Explorer** window, right-click the solution, and then click **Add Solution to Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > <span data-ttu-id="1ed33-149">In alcuni casi, a seconda del modo in cui l'organizzazione mi piace al contenuto della struttura in TFS, devi aggiungere progetti al controllo del codice sorgente singolarmente per fornire un controllo più accurato sul modo in cui è organizzato il codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="1ed33-149">In some cases, depending on how your organization likes to structure content in TFS, you may need to add projects to source control individually to provide more fine-grained control over how your source code is organized.</span></span>
4. <span data-ttu-id="1ed33-150">Verificare che il **Esplora controllo codice sorgente** scheda viene visualizzato il contenuto è stato aggiunto all'interno della struttura di cartelle del server per il progetto team.</span><span class="sxs-lookup"><span data-stu-id="1ed33-150">Verify that the **Source Control Explorer** tab displays the content you've added within the server folder structure for the team project.</span></span>

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > <span data-ttu-id="1ed33-151">Il **Esplora controllo codice sorgente** scheda consente di visualizzare i contenuti con la richiesta alcuna ulteriore perché è stata aggiunta la soluzione in una cartella mappata nel file system locale.</span><span class="sxs-lookup"><span data-stu-id="1ed33-151">The **Source Control Explorer** tab displays your content with no further prompting because you added your solution to a mapped folder on the local file system.</span></span> <span data-ttu-id="1ed33-152">Se la soluzione in una posizione non mappata, sarebbe chiesto di specificare percorsi di cartelle in TFS e nel file system locale.</span><span class="sxs-lookup"><span data-stu-id="1ed33-152">If your solution was in an unmapped location, you'd be prompted to specify folder locations in both TFS and your local file system.</span></span>
5. <span data-ttu-id="1ed33-153">Nel **Esplora controllo codice sorgente** nella scheda il **cartelle** riquadro, fare clic sul progetto team (ad esempio, **ContactManager**) e quindi fare clic su **Check-In Le modifiche in sospeso**.</span><span class="sxs-lookup"><span data-stu-id="1ed33-153">On the **Source Control Explorer** tab, in the **Folders** pane, right-click the team project (for example, **ContactManager**), and then click **Check In Pending Changes**.</span></span>
6. <span data-ttu-id="1ed33-154">Nel **Check-In-file di origine** della finestra di dialogo digitare un commento e quindi fare clic su **Archivia**.</span><span class="sxs-lookup"><span data-stu-id="1ed33-154">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

    ![](adding-content-to-source-control/_static/image9.png)

<span data-ttu-id="1ed33-155">A questo punto è stato aggiunto la soluzione al controllo del codice sorgente in TFS.</span><span class="sxs-lookup"><span data-stu-id="1ed33-155">At this point you have added your solution to source control in TFS.</span></span>

## <a name="add-external-dependencies-to-source-control"></a><span data-ttu-id="1ed33-156">Aggiungere le dipendenze esterne a controllo del codice sorgente</span><span class="sxs-lookup"><span data-stu-id="1ed33-156">Add External Dependencies to Source Control</span></span>

<span data-ttu-id="1ed33-157">Quando si aggiunge un progetto o una soluzione al controllo del codice sorgente, eventuali file e cartelle all'interno del progetto o soluzione verranno inoltre aggiunto.</span><span class="sxs-lookup"><span data-stu-id="1ed33-157">When you add a project or solution to source control, any files and folders within your project or solution will also be added.</span></span> <span data-ttu-id="1ed33-158">Tuttavia, in molti casi, progetti e soluzioni anche basano su dipendenze esterne, ad esempio assembly locali, per funzionare correttamente.</span><span class="sxs-lookup"><span data-stu-id="1ed33-158">However, in a lot of cases, projects and solutions also rely on external dependencies, like local assemblies, to function properly.</span></span> <span data-ttu-id="1ed33-159">È necessario aggiungere tutte le risorse di controllo del codice sorgente per consentire a Team Build e altri membri del team di sviluppo compilare il codice completato.</span><span class="sxs-lookup"><span data-stu-id="1ed33-159">You need to add any such resources to source control to let both Team Build and other members of the developer team build your code successfully.</span></span>

<span data-ttu-id="1ed33-160">Ad esempio, la struttura di cartelle per la soluzione di esempio Contact Manager include una cartella denominata dei pacchetti.</span><span class="sxs-lookup"><span data-stu-id="1ed33-160">For example, the folder structure for the Contact Manager sample solution includes a folder named packages.</span></span> <span data-ttu-id="1ed33-161">Contiene l'assembly e varie risorse di supporto per ADO.NET Entity Framework 4.1.</span><span class="sxs-lookup"><span data-stu-id="1ed33-161">This contains the assembly and various supporting resources for the ADO.NET Entity Framework 4.1.</span></span> <span data-ttu-id="1ed33-162">La cartella dei pacchetti non fa parte della soluzione Contact Manager, ma la soluzione non verrà compilato correttamente senza di essa.</span><span class="sxs-lookup"><span data-stu-id="1ed33-162">The packages folder is not part of the Contact Manager solution, but the solution will not build successfully without it.</span></span> <span data-ttu-id="1ed33-163">Per abilitare il Team Build compilare la soluzione, è necessario aggiungere la cartella di pacchetti al controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="1ed33-163">To enable Team Build to build the solution, you need to add the packages folder to source control.</span></span>

> [!NOTE]
> <span data-ttu-id="1ed33-164">L'inclusione di una cartella di pacchetti è tipico di ciò che accade quando si aggiunge di Entity Framework, o risorse simili, alla soluzione mediante l'estensione NuGet per Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="1ed33-164">The inclusion of a packages folder is typical of what happens when you add the Entity Framework, or similar resources, to your solution using the NuGet extension for Visual Studio 2010.</span></span>


<span data-ttu-id="1ed33-165">**Per aggiungere contenuto non di progetto al controllo del codice sorgente**</span><span class="sxs-lookup"><span data-stu-id="1ed33-165">**To add non-project content to source control**</span></span>

1. <span data-ttu-id="1ed33-166">Assicurarsi che gli elementi da aggiungere (ad esempio, la cartella dei pacchetti) siano in una posizione appropriata all'interno di una cartella mappata nel file system locale.</span><span class="sxs-lookup"><span data-stu-id="1ed33-166">Ensure that the items you want to add (for example, the packages folder) are in an appropriate location within a mapped folder on your local file system.</span></span>
2. <span data-ttu-id="1ed33-167">In Visual Studio 2010, nelle **Team Explorer** finestra, espandere il progetto team e quindi fare doppio clic su **controllo del codice sorgente**.</span><span class="sxs-lookup"><span data-stu-id="1ed33-167">In Visual Studio 2010, In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image10.png)
3. <span data-ttu-id="1ed33-168">Nel **Esplora controllo codice sorgente** nella scheda il **cartelle** riquadro, selezionare la cartella che contiene l'elemento o gli elementi si desidera aggiungere.</span><span class="sxs-lookup"><span data-stu-id="1ed33-168">On the **Source Control Explorer** tab, in the **Folders** pane, select the folder that contains the item or items you want to add.</span></span>
4. <span data-ttu-id="1ed33-169">Scegliere il **Aggiungi elementi alla cartella** pulsante.</span><span class="sxs-lookup"><span data-stu-id="1ed33-169">Click the **Add Items to Folder** button.</span></span>

    ![](adding-content-to-source-control/_static/image11.png)
5. <span data-ttu-id="1ed33-170">Nel **aggiungere al controllo del codice sorgente** finestra di dialogo, selezionare la cartella o elementi che si desidera aggiungere e quindi fare clic su **successivo**.</span><span class="sxs-lookup"><span data-stu-id="1ed33-170">In the **Add to Source Control** dialog box, select the folder or items you want to add, and then click **Next**.</span></span>

    ![](adding-content-to-source-control/_static/image12.png)
6. <span data-ttu-id="1ed33-171">Nel **esclusi gli elementi** scheda, selezionare gli elementi necessari sono stati esclusi automaticamente (ad esempio, gli assembly) e quindi fare clic su **Includi elementi**.</span><span class="sxs-lookup"><span data-stu-id="1ed33-171">On the **Excluded items** tab, select any required items that have been automatically excluded (for example, assemblies), and then click **Include item(s)**.</span></span>

    ![](adding-content-to-source-control/_static/image13.png)
7. <span data-ttu-id="1ed33-172">Nel **elementi da aggiungere** scheda, verificare che siano elencati tutti i file da includere e quindi fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="1ed33-172">On the **Items to add** tab, verify that all the files you want to include are listed, and then click **Finish**.</span></span>

    ![](adding-content-to-source-control/_static/image14.png)
8. <span data-ttu-id="1ed33-173">Nel **Esplora controllo codice sorgente** finestra, fare clic sui **Archivia** pulsante.</span><span class="sxs-lookup"><span data-stu-id="1ed33-173">In the **Source Control Explorer** window, click the **Check In** button.</span></span>

    ![](adding-content-to-source-control/_static/image15.png)
9. <span data-ttu-id="1ed33-174">Nel **Check-In-file di origine** della finestra di dialogo digitare un commento e quindi fare clic su **Archivia**.</span><span class="sxs-lookup"><span data-stu-id="1ed33-174">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

<span data-ttu-id="1ed33-175">A questo punto, si sono aggiunte le dipendenze esterne per la soluzione al controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="1ed33-175">At this point, you have added the external dependencies for your solution to source control.</span></span>

## <a name="conclusion"></a><span data-ttu-id="1ed33-176">Conclusione</span><span class="sxs-lookup"><span data-stu-id="1ed33-176">Conclusion</span></span>

<span data-ttu-id="1ed33-177">In questo argomento descrive come connettersi a un progetto team, eseguire il mapping di una struttura di cartelle e aggiungere contenuto al controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="1ed33-177">This topic described how to connect to a team project, map a folder structure, and add content to source control.</span></span> <span data-ttu-id="1ed33-178">Per altre informazioni su come lavorare con gli elementi nel controllo del codice sorgente, vedere [tramite controllo della versione](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="1ed33-178">For more information on how to work with items under source control, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

<span data-ttu-id="1ed33-179">Argomento successivo [configurazione di un Server di compilazione TFS per la distribuzione Web](configuring-a-tfs-build-server-for-web-deployment.md), viene descritto come preparare un server TFS Team Build per compilare e distribuire la soluzione.</span><span class="sxs-lookup"><span data-stu-id="1ed33-179">The next topic, [Configuring a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md), describes how to prepare a TFS Team Build server to build and deploy your solution.</span></span>

## <a name="further-reading"></a><span data-ttu-id="1ed33-180">Ulteriori informazioni</span><span class="sxs-lookup"><span data-stu-id="1ed33-180">Further Reading</span></span>

<span data-ttu-id="1ed33-181">Per informazioni più complete sull'uso di controllo del codice sorgente in TFS, vedere [tramite controllo della versione](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="1ed33-181">For more comprehensive information on working with source control in TFS, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1ed33-182">[Precedente](creating-a-team-project-in-tfs.md)
> [Successivo](configuring-a-tfs-build-server-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="1ed33-182">[Previous](creating-a-team-project-in-tfs.md)
[Next](configuring-a-tfs-build-server-for-web-deployment.md)</span></span>
