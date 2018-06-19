---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: Aggiunta di contenuto al controllo del codice sorgente | Documenti Microsoft
author: jrjlee
description: In questo argomento viene illustrato come aggiungere contenuto al controllo del codice sorgente in Team Foundation Server (TFS) 2010. Viene descritto come aggiungere soluzioni e progetti per un progetto team...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: c9c3a506d2745a6793661453a293732429d3e46e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890434"
---
<a name="adding-content-to-source-control"></a><span data-ttu-id="005ef-104">Aggiunta di contenuto al controllo del codice sorgente</span><span class="sxs-lookup"><span data-stu-id="005ef-104">Adding Content to Source Control</span></span>
====================
<span data-ttu-id="005ef-105">da [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="005ef-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="005ef-106">Scarica il PDF</span><span class="sxs-lookup"><span data-stu-id="005ef-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="005ef-107">In questo argomento viene illustrato come aggiungere contenuto al controllo del codice sorgente in Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="005ef-107">This topic explains how to add content to source control in Team Foundation Server (TFS) 2010.</span></span> <span data-ttu-id="005ef-108">Viene descritto come aggiungere soluzioni e progetti in un progetto team in TFS e viene descritto come aggiungere dipendenze esterne come Framework o assembly di controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="005ef-108">It describes how to add solutions and projects to a team project in TFS, and it explains how to add external dependencies like frameworks or assemblies to source control.</span></span>


<span data-ttu-id="005ef-109">In questo argomento fa parte di una serie di esercitazioni basate su requisiti di distribuzione dell'organizzazione di una società fittizia denominata Fabrikam, Inc. Questa serie di esercitazioni Usa una soluzione di esempio&#x2014;il [Contact Manager soluzione](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;per rappresentare un'applicazione web con un livello di complessità, tra cui un'applicazione ASP.NET MVC 3, una comunicazione Windows realistico Servizio Foundation (WCF) e un progetto di database.</span><span class="sxs-lookup"><span data-stu-id="005ef-109">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="005ef-110">Panoramica di Task</span><span class="sxs-lookup"><span data-stu-id="005ef-110">Task Overview</span></span>

<span data-ttu-id="005ef-111">Nella maggior parte dei casi, ogni membro del team di sviluppo deve essere in grado di aggiungere contenuto al controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="005ef-111">In most cases, every member of the developer team should be able to add content to source control.</span></span> <span data-ttu-id="005ef-112">Per aggiungere una soluzione al controllo del codice sorgente in TFS, è necessario completare questi passaggi di alto livello:</span><span class="sxs-lookup"><span data-stu-id="005ef-112">To add a solution to source control in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="005ef-113">Connettersi a un progetto team.</span><span class="sxs-lookup"><span data-stu-id="005ef-113">Connect to a team project.</span></span>
- <span data-ttu-id="005ef-114">Mappare la struttura di cartelle del progetto team sul server a una struttura di cartelle nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="005ef-114">Map the team project folder structure on the server to a folder structure on your local computer.</span></span>
- <span data-ttu-id="005ef-115">Controllo del codice sorgente, aggiungere la soluzione e il relativo contenuto.</span><span class="sxs-lookup"><span data-stu-id="005ef-115">Add the solution and its contents to source control.</span></span>
- <span data-ttu-id="005ef-116">Aggiungere le eventuali dipendenze esterne a controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="005ef-116">Add any external dependencies to source control.</span></span>

<span data-ttu-id="005ef-117">Questo argomento viene illustrato come eseguire queste procedure.</span><span class="sxs-lookup"><span data-stu-id="005ef-117">This topic will show you how to perform these procedures.</span></span>

<span data-ttu-id="005ef-118">Le attività e procedure dettagliate in questo argomento si presuppongono che è già stato creato un nuovo progetto team TFS per gestire il contenuto.</span><span class="sxs-lookup"><span data-stu-id="005ef-118">The tasks and walkthroughs in this topic assume that you've already created a new TFS team project to manage your content.</span></span> <span data-ttu-id="005ef-119">Per ulteriori informazioni sulla creazione di un nuovo progetto team, vedere [la creazione di un progetto Team in TFS](creating-a-team-project-in-tfs.md).</span><span class="sxs-lookup"><span data-stu-id="005ef-119">For more information on creating a new team project, see [Creating a Team Project in TFS](creating-a-team-project-in-tfs.md).</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="005ef-120">Che consente di eseguire queste procedure?</span><span class="sxs-lookup"><span data-stu-id="005ef-120">Who Performs These Procedures?</span></span>

<span data-ttu-id="005ef-121">Nella maggior parte dei casi, ogni membro del team di sviluppo deve essere in grado di aggiungere e modificare il contenuto all'interno dei progetti team specifico.</span><span class="sxs-lookup"><span data-stu-id="005ef-121">In most cases, every member of the developer team should be able to add and modify content within specific team projects.</span></span>

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a><span data-ttu-id="005ef-122">Connettersi a un progetto Team e creare un Mapping della cartella</span><span class="sxs-lookup"><span data-stu-id="005ef-122">Connect to a Team Project and Create a Folder Mapping</span></span>

<span data-ttu-id="005ef-123">Prima di aggiungere contenuto al controllo del codice sorgente, è necessario connettersi a un progetto team e creare un mapping tra la struttura di cartelle nel server e il file system nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="005ef-123">Before you add any content to source control, you need to connect to a team project and create a mapping between the folder structure on the server and the file system on your local machine.</span></span>

<span data-ttu-id="005ef-124">**Per connettersi a un progetto team ed eseguire il mapping di un percorso locale**</span><span class="sxs-lookup"><span data-stu-id="005ef-124">**To connect to a team project and map a local path**</span></span>

1. <span data-ttu-id="005ef-125">Nella workstation di sviluppo, aprire Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="005ef-125">On your developer workstation, open Visual Studio 2010.</span></span>
2. <span data-ttu-id="005ef-126">In Visual Studio, sul **Team** menu, fare clic su **Connetti a Team Foundation Server**.</span><span class="sxs-lookup"><span data-stu-id="005ef-126">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="005ef-127">Se già stata configurata una connessione a un server TFS, è possibile omettere i passaggi da 3 a 6.</span><span class="sxs-lookup"><span data-stu-id="005ef-127">If you have already configured a connection to a TFS server, you can omit steps 3-6.</span></span>
3. <span data-ttu-id="005ef-128">Nel **connessione al progetto Team** la finestra di dialogo, fare clic su **server**.</span><span class="sxs-lookup"><span data-stu-id="005ef-128">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
4. <span data-ttu-id="005ef-129">Nel **Aggiungi/Rimuovi Team Foundation Server** la finestra di dialogo, fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="005ef-129">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
5. <span data-ttu-id="005ef-130">Nel **Aggiungi Team Foundation Server** la finestra di dialogo, fornire i dettagli dell'istanza di TFS, quindi fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="005ef-130">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](adding-content-to-source-control/_static/image1.png)
6. <span data-ttu-id="005ef-131">Nel **Aggiungi/Rimuovi Team Foundation Server** la finestra di dialogo, fare clic su **Chiudi**.</span><span class="sxs-lookup"><span data-stu-id="005ef-131">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
7. <span data-ttu-id="005ef-132">Nel **Connetti a progetto Team** nella finestra di dialogo selezionare l'istanza TFS si desidera connettersi, per selezionare il team di progetto di raccolta, selezionare il progetto team si desidera aggiungere e quindi fare clic su **Connetti**.</span><span class="sxs-lookup"><span data-stu-id="005ef-132">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection, select the team project you want to add to, and then click **Connect**.</span></span>

    ![](adding-content-to-source-control/_static/image2.png)
8. <span data-ttu-id="005ef-133">Nel **Team Explorer** finestra, espandere il progetto team e quindi fare doppio clic su **controllo del codice sorgente**.</span><span class="sxs-lookup"><span data-stu-id="005ef-133">In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image3.png)
9. <span data-ttu-id="005ef-134">Nel **Esplora controllo codice sorgente** scheda, fare clic su **non mappato**.</span><span class="sxs-lookup"><span data-stu-id="005ef-134">On the **Source Control Explorer** tab, click **Not mapped**.</span></span>

    ![](adding-content-to-source-control/_static/image4.png)
10. <span data-ttu-id="005ef-135">Nel **mappa** della finestra di dialogo di **cartella locale** casella, selezionare (o creare) una cartella locale per agire come la cartella radice per il progetto team e quindi fare clic su **mappa**.</span><span class="sxs-lookup"><span data-stu-id="005ef-135">In the **Map** dialog box, in the **Local folder** box, browse to (or create) a local folder to act as the root folder for the team project, and then click **Map**.</span></span>

    ![](adding-content-to-source-control/_static/image5.png)
11. <span data-ttu-id="005ef-136">Quando viene chiesto di scaricare i file di origine, fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="005ef-136">When you're prompted to download source files, click **Yes**.</span></span>

    ![](adding-content-to-source-control/_static/image6.png)

<span data-ttu-id="005ef-137">A questo punto, è stato eseguito il mapping di cartella sul lato server per il progetto team in una cartella locale nella workstation di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="005ef-137">At this point, you have mapped the server-side folder for the team project to a local folder on your developer workstation.</span></span> <span data-ttu-id="005ef-138">Una volta anche scaricato qualsiasi contenuto esistente dal progetto team per la struttura di cartelle locali.</span><span class="sxs-lookup"><span data-stu-id="005ef-138">You've also downloaded any existing content from the team project to your local folder structure.</span></span> <span data-ttu-id="005ef-139">È ora possibile iniziare a aggiungere i propri dati al controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="005ef-139">You can now start to add your own content to source control.</span></span>

## <a name="add-projects-and-solutions-to-source-control"></a><span data-ttu-id="005ef-140">Aggiungere soluzioni e progetti al controllo del codice sorgente</span><span class="sxs-lookup"><span data-stu-id="005ef-140">Add Projects and Solutions to Source Control</span></span>

<span data-ttu-id="005ef-141">Per aggiungere soluzioni e progetti al controllo del codice sorgente, è innanzitutto necessario spostarli nella cartella per il progetto team mappato nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="005ef-141">To add projects and solutions to source control, you first need to move them to the mapped folder for the team project on your local machine.</span></span> <span data-ttu-id="005ef-142">È quindi possibile archiviare il contenuto da sincronizzare le aggiunte con il server.</span><span class="sxs-lookup"><span data-stu-id="005ef-142">You can then check in the content to synchronize your additions with the server.</span></span>

<span data-ttu-id="005ef-143">**Per aggiungere progetti al controllo del codice sorgente**</span><span class="sxs-lookup"><span data-stu-id="005ef-143">**To add projects to source control**</span></span>

1. <span data-ttu-id="005ef-144">Nella workstation di sviluppo, spostare i progetti e soluzioni in una posizione appropriata all'interno della struttura di cartella mappata per il progetto team.</span><span class="sxs-lookup"><span data-stu-id="005ef-144">On your developer workstation, move your projects and solutions to an appropriate location within the mapped folder structure for the team project.</span></span>

    > [!NOTE]
    > <span data-ttu-id="005ef-145">Molte organizzazioni disporrà di un approccio consigliato per come progetti e soluzioni devono essere organizzate nel controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="005ef-145">Many organizations will have a preferred approach to how projects and solutions should be organized in source control.</span></span> <span data-ttu-id="005ef-146">Per istruzioni su come strutturare delle cartelle, vedere [How To: struttura del controllo cartelle del codice sorgente in Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span><span class="sxs-lookup"><span data-stu-id="005ef-146">For guidance on how to structure folders, see [How To: Structure Your Source Control Folders in Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).</span></span>
2. <span data-ttu-id="005ef-147">Aprire la soluzione in Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="005ef-147">Open the solution in Visual Studio 2010.</span></span>
3. <span data-ttu-id="005ef-148">Nel **Esplora** finestra rapida della soluzione e quindi fare clic su **Aggiungi soluzione al controllo del codice sorgente**.</span><span class="sxs-lookup"><span data-stu-id="005ef-148">In the **Solution Explorer** window, right-click the solution, and then click **Add Solution to Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > <span data-ttu-id="005ef-149">In alcuni casi, a seconda di come l'organizzazione ama contenuto della struttura in TFS, devi aggiungere progetti al controllo del codice sorgente singolarmente per fornire un controllo più accurato tramite la modalità di organizzazione del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="005ef-149">In some cases, depending on how your organization likes to structure content in TFS, you may need to add projects to source control individually to provide more fine-grained control over how your source code is organized.</span></span>
4. <span data-ttu-id="005ef-150">Verificare che il **Esplora controllo codice sorgente** scheda consente di visualizzare il contenuto è stato aggiunto all'interno della struttura di cartelle del server per il progetto team.</span><span class="sxs-lookup"><span data-stu-id="005ef-150">Verify that the **Source Control Explorer** tab displays the content you've added within the server folder structure for the team project.</span></span>

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > <span data-ttu-id="005ef-151">Il **Esplora controllo codice sorgente** scheda consente di visualizzare il contenuto di una richiesta di conferma senza ulteriore poiché la soluzione è stato aggiunto a una cartella mappata nel file system locale.</span><span class="sxs-lookup"><span data-stu-id="005ef-151">The **Source Control Explorer** tab displays your content with no further prompting because you added your solution to a mapped folder on the local file system.</span></span> <span data-ttu-id="005ef-152">Se la soluzione in una posizione non mappata, verrà chiesto di specificare i percorsi delle cartelle in TFS e file system locale.</span><span class="sxs-lookup"><span data-stu-id="005ef-152">If your solution was in an unmapped location, you'd be prompted to specify folder locations in both TFS and your local file system.</span></span>
5. <span data-ttu-id="005ef-153">Nel **Esplora controllo codice sorgente** nella scheda il **cartelle** riquadro, fare clic sul progetto team (ad esempio, **ContactManager**) e quindi fare clic su **archivia Le modifiche in sospeso**.</span><span class="sxs-lookup"><span data-stu-id="005ef-153">On the **Source Control Explorer** tab, in the **Folders** pane, right-click the team project (for example, **ContactManager**), and then click **Check In Pending Changes**.</span></span>
6. <span data-ttu-id="005ef-154">Nel **Check-In-file di origine** nella finestra di dialogo digitare un commento e quindi fare clic su **Archivia**.</span><span class="sxs-lookup"><span data-stu-id="005ef-154">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

    ![](adding-content-to-source-control/_static/image9.png)

<span data-ttu-id="005ef-155">A questo punto è stato aggiunto la soluzione al controllo del codice sorgente in TFS.</span><span class="sxs-lookup"><span data-stu-id="005ef-155">At this point you have added your solution to source control in TFS.</span></span>

## <a name="add-external-dependencies-to-source-control"></a><span data-ttu-id="005ef-156">Aggiungere le dipendenze esterne a controllo del codice sorgente</span><span class="sxs-lookup"><span data-stu-id="005ef-156">Add External Dependencies to Source Control</span></span>

<span data-ttu-id="005ef-157">Quando si aggiunge un progetto o una soluzione a controllo del codice sorgente, tutti i file e cartelle all'interno del progetto o soluzione verranno inoltre aggiunto.</span><span class="sxs-lookup"><span data-stu-id="005ef-157">When you add a project or solution to source control, any files and folders within your project or solution will also be added.</span></span> <span data-ttu-id="005ef-158">Tuttavia, in molti casi, progetti e soluzioni anche basano su dipendenze esterne, ad esempio assembly locale, per funzionare correttamente.</span><span class="sxs-lookup"><span data-stu-id="005ef-158">However, in a lot of cases, projects and solutions also rely on external dependencies, like local assemblies, to function properly.</span></span> <span data-ttu-id="005ef-159">È necessario aggiungere tutte le risorse di controllo del codice sorgente per consentire di Team Build e gli altri membri del team di sviluppatori di compilare il codice correttamente.</span><span class="sxs-lookup"><span data-stu-id="005ef-159">You need to add any such resources to source control to let both Team Build and other members of the developer team build your code successfully.</span></span>

<span data-ttu-id="005ef-160">Ad esempio, la struttura di cartelle per la soluzione di esempio Contact Manager include una cartella denominata pacchetti.</span><span class="sxs-lookup"><span data-stu-id="005ef-160">For example, the folder structure for the Contact Manager sample solution includes a folder named packages.</span></span> <span data-ttu-id="005ef-161">Contiene l'assembly e varie risorse di supporto per ADO.NET Entity Framework 4.1.</span><span class="sxs-lookup"><span data-stu-id="005ef-161">This contains the assembly and various supporting resources for the ADO.NET Entity Framework 4.1.</span></span> <span data-ttu-id="005ef-162">La cartella di pacchetti non è parte della soluzione di gestione di contatto, ma la soluzione non verrà compilato correttamente senza di esso.</span><span class="sxs-lookup"><span data-stu-id="005ef-162">The packages folder is not part of the Contact Manager solution, but the solution will not build successfully without it.</span></span> <span data-ttu-id="005ef-163">Per abilitare Team Build compilare la soluzione, è necessario aggiungere la cartella di pacchetti al controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="005ef-163">To enable Team Build to build the solution, you need to add the packages folder to source control.</span></span>

> [!NOTE]
> <span data-ttu-id="005ef-164">L'inclusione di una cartella di pacchetti è tipico di ciò che accade quando si aggiunge la Entity Framework, o le risorse simile, alla soluzione mediante l'estensione NuGet per Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="005ef-164">The inclusion of a packages folder is typical of what happens when you add the Entity Framework, or similar resources, to your solution using the NuGet extension for Visual Studio 2010.</span></span>


<span data-ttu-id="005ef-165">**Per aggiungere contenuto non di progetto al controllo del codice sorgente**</span><span class="sxs-lookup"><span data-stu-id="005ef-165">**To add non-project content to source control**</span></span>

1. <span data-ttu-id="005ef-166">Verificare che gli elementi che si desidera aggiungere (ad esempio, la cartella pacchetti) siano in una posizione appropriata all'interno di una cartella mappata nel file system locale.</span><span class="sxs-lookup"><span data-stu-id="005ef-166">Ensure that the items you want to add (for example, the packages folder) are in an appropriate location within a mapped folder on your local file system.</span></span>
2. <span data-ttu-id="005ef-167">In Visual Studio 2010, nel **Team Explorer** finestra, espandere il progetto team e quindi fare doppio clic su **controllo del codice sorgente**.</span><span class="sxs-lookup"><span data-stu-id="005ef-167">In Visual Studio 2010, In the **Team Explorer** window, expand your team project, and then double-click **Source Control**.</span></span>

    ![](adding-content-to-source-control/_static/image10.png)
3. <span data-ttu-id="005ef-168">Nel **Esplora controllo codice sorgente** nella scheda il **cartelle** riquadro, selezionare la cartella che contiene l'elemento o gli elementi si desidera aggiungere.</span><span class="sxs-lookup"><span data-stu-id="005ef-168">On the **Source Control Explorer** tab, in the **Folders** pane, select the folder that contains the item or items you want to add.</span></span>
4. <span data-ttu-id="005ef-169">Fare clic su di **Aggiungi elementi alla cartella** pulsante.</span><span class="sxs-lookup"><span data-stu-id="005ef-169">Click the **Add Items to Folder** button.</span></span>

    ![](adding-content-to-source-control/_static/image11.png)
5. <span data-ttu-id="005ef-170">Nel **aggiungere al controllo del codice sorgente** finestra di dialogo, selezionare la cartella o elementi che si desidera aggiungere e quindi fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="005ef-170">In the **Add to Source Control** dialog box, select the folder or items you want to add, and then click **Next**.</span></span>

    ![](adding-content-to-source-control/_static/image12.png)
6. <span data-ttu-id="005ef-171">Nel **tutti gli elementi** scheda, selezionare gli elementi necessari sono stati esclusi automaticamente (ad esempio, assembly) e quindi fare clic su **includono elementi**.</span><span class="sxs-lookup"><span data-stu-id="005ef-171">On the **Excluded items** tab, select any required items that have been automatically excluded (for example, assemblies), and then click **Include item(s)**.</span></span>

    ![](adding-content-to-source-control/_static/image13.png)
7. <span data-ttu-id="005ef-172">Nel **elementi da aggiungere** scheda, verificare che sono elencati tutti i file che si desidera includere e quindi fare clic su **fine**.</span><span class="sxs-lookup"><span data-stu-id="005ef-172">On the **Items to add** tab, verify that all the files you want to include are listed, and then click **Finish**.</span></span>

    ![](adding-content-to-source-control/_static/image14.png)
8. <span data-ttu-id="005ef-173">Nel **Esplora controllo codice sorgente** finestra, fare clic su di **Archivia** pulsante.</span><span class="sxs-lookup"><span data-stu-id="005ef-173">In the **Source Control Explorer** window, click the **Check In** button.</span></span>

    ![](adding-content-to-source-control/_static/image15.png)
9. <span data-ttu-id="005ef-174">Nel **Check-In-file di origine** nella finestra di dialogo digitare un commento e quindi fare clic su **Archivia**.</span><span class="sxs-lookup"><span data-stu-id="005ef-174">In the **Check In – Source Files** dialog box, type a comment, and then click **Check In**.</span></span>

<span data-ttu-id="005ef-175">A questo punto, è stato aggiunto le dipendenze esterne per la soluzione al controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="005ef-175">At this point, you have added the external dependencies for your solution to source control.</span></span>

## <a name="conclusion"></a><span data-ttu-id="005ef-176">Conclusione</span><span class="sxs-lookup"><span data-stu-id="005ef-176">Conclusion</span></span>

<span data-ttu-id="005ef-177">In questo argomento viene descritto come connettersi a un progetto team, eseguire il mapping di una struttura di cartelle e aggiungere contenuto al controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="005ef-177">This topic described how to connect to a team project, map a folder structure, and add content to source control.</span></span> <span data-ttu-id="005ef-178">Per ulteriori informazioni su come lavorare con gli elementi nel controllo del codice sorgente, vedere [uso del controllo della versione](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="005ef-178">For more information on how to work with items under source control, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

<span data-ttu-id="005ef-179">L'argomento successivo, [configurazione di un Server di compilazione TFS per la distribuzione Web](configuring-a-tfs-build-server-for-web-deployment.md), viene descritto come preparare un server TFS Team Build per compilare e distribuire la soluzione.</span><span class="sxs-lookup"><span data-stu-id="005ef-179">The next topic, [Configuring a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md), describes how to prepare a TFS Team Build server to build and deploy your solution.</span></span>

## <a name="further-reading"></a><span data-ttu-id="005ef-180">Ulteriori informazioni</span><span class="sxs-lookup"><span data-stu-id="005ef-180">Further Reading</span></span>

<span data-ttu-id="005ef-181">Per ulteriori informazioni sull'uso di controllo del codice sorgente in TFS, vedere [uso del controllo della versione](https://msdn.microsoft.com/library/ms181368.aspx).</span><span class="sxs-lookup"><span data-stu-id="005ef-181">For more comprehensive information on working with source control in TFS, see [Using Version Control](https://msdn.microsoft.com/library/ms181368.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="005ef-182">[Precedente](creating-a-team-project-in-tfs.md)
> [Successivo](configuring-a-tfs-build-server-for-web-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="005ef-182">[Previous](creating-a-team-project-in-tfs.md)
[Next](configuring-a-tfs-build-server-for-web-deployment.md)</span></span>
