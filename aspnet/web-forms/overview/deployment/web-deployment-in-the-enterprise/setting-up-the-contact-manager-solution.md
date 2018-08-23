---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: Impostazione della soluzione Contact Manager | Microsoft Docs
author: jrjlee
description: In questo argomento viene descritto come scaricare e configurare la soluzione Contact Manager venga eseguita in locale in una workstation di sviluppo.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: d834e6fbd2b46dca8bd7eeadc1eb68b33e62bb38
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838632"
---
<a name="setting-up-the-contact-manager-solution"></a><span data-ttu-id="e0428-103">Impostazione della soluzione Contact Manager</span><span class="sxs-lookup"><span data-stu-id="e0428-103">Setting Up the Contact Manager Solution</span></span>
====================
<span data-ttu-id="e0428-104">da [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="e0428-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="e0428-105">Scaricare PDF</span><span class="sxs-lookup"><span data-stu-id="e0428-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="e0428-106">In questo argomento viene descritto come scaricare e configurare la soluzione Contact Manager venga eseguita in locale in una workstation di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="e0428-106">This topic describes how to download and configure the Contact Manager solution to run locally on a developer workstation.</span></span>


## <a name="system-requirements"></a><span data-ttu-id="e0428-107">Requisiti di sistema</span><span class="sxs-lookup"><span data-stu-id="e0428-107">System Requirements</span></span>

<span data-ttu-id="e0428-108">Per eseguire la soluzione Contact Manager in locale ed eseguire altre attività descritte in questa esercitazione, è necessario installarvi il software della workstation dello sviluppatore:</span><span class="sxs-lookup"><span data-stu-id="e0428-108">To run the Contact Manager solution locally and to perform the other tasks described in this tutorial, you'll need to install this software on your developer workstation:</span></span>

- <span data-ttu-id="e0428-109">Visual Studio 2010 Service Pack 1, Premium o Ultimate Edition</span><span class="sxs-lookup"><span data-stu-id="e0428-109">Visual Studio 2010 Service Pack 1, Premium or Ultimate Edition</span></span>
- <span data-ttu-id="e0428-110">Internet Information Services (IIS) 7.5 Express</span><span class="sxs-lookup"><span data-stu-id="e0428-110">Internet Information Services (IIS) 7.5 Express</span></span>
- <span data-ttu-id="e0428-111">SQL Server Express 2008 R2</span><span class="sxs-lookup"><span data-stu-id="e0428-111">SQL Server Express 2008 R2</span></span>
- <span data-ttu-id="e0428-112">IIS strumento di distribuzione Web (distribuzione Web) 2.1 o versioni successive</span><span class="sxs-lookup"><span data-stu-id="e0428-112">IIS Web Deployment Tool (Web Deploy) 2.1 or later</span></span>
- <span data-ttu-id="e0428-113">ASP.NET 4.0</span><span class="sxs-lookup"><span data-stu-id="e0428-113">ASP.NET 4.0</span></span>
- <span data-ttu-id="e0428-114">ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="e0428-114">ASP.NET MVC 3</span></span>
- <span data-ttu-id="e0428-115">.NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="e0428-115">.NET Framework 4</span></span>
- <span data-ttu-id="e0428-116">.NET Framework 3.5 SP1</span><span class="sxs-lookup"><span data-stu-id="e0428-116">.NET Framework 3.5 SP1</span></span>

<span data-ttu-id="e0428-117">Fatta eccezione per Visual Studio 2010, è possibile scaricare e installare le versioni più recenti di questi prodotti e i componenti tramite il [instalace Webové Platformy](https://go.microsoft.com/?linkid=9805118).</span><span class="sxs-lookup"><span data-stu-id="e0428-117">With the exception of Visual Studio 2010, you can download and install the latest versions of all of these products and components through the [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).</span></span>

## <a name="download-and-extract-the-solution"></a><span data-ttu-id="e0428-118">Scaricare ed estrarre la soluzione</span><span class="sxs-lookup"><span data-stu-id="e0428-118">Download and Extract the Solution</span></span>

<span data-ttu-id="e0428-119">È possibile scaricare l'applicazione di esempio Contact Manager da MSDN Code Gallery [qui](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).</span><span class="sxs-lookup"><span data-stu-id="e0428-119">You can download the Contact Manager sample application from the MSDN Code Gallery [here](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).</span></span>

## <a name="configure-and-run-the-solution"></a><span data-ttu-id="e0428-120">Configurare ed eseguire la soluzione</span><span class="sxs-lookup"><span data-stu-id="e0428-120">Configure and Run the Solution</span></span>

<span data-ttu-id="e0428-121">Per configurare ed eseguire la soluzione Contact Manager nel computer locale, è necessario eseguire i passaggi generali:</span><span class="sxs-lookup"><span data-stu-id="e0428-121">To configure and run the Contact Manager solution on your local machine, you'll need to perform these high-level steps:</span></span>

1. <span data-ttu-id="e0428-122">Se non è già, creare un database di servizi dell'applicazione ASP.NET locale con le funzionalità di gestione di appartenenza e ruoli abilitate.</span><span class="sxs-lookup"><span data-stu-id="e0428-122">If you don't have one already, create a local ASP.NET application services database with the membership and role management features enabled.</span></span>
2. <span data-ttu-id="e0428-123">Modificare le stringhe di connessione nel *Web. config* in modo che puntino all'istanza locale di SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="e0428-123">Edit connection strings in the *web.config* files to point to your local SQL Server Express instance.</span></span>
3. <span data-ttu-id="e0428-124">Eseguire la soluzione da Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="e0428-124">Run the solution from Visual Studio 2010.</span></span>

<span data-ttu-id="e0428-125">Il resto di questa sezione fornisce altre indicazioni su come completare queste attività.</span><span class="sxs-lookup"><span data-stu-id="e0428-125">The remainder of this section provides more guidance on how to complete each of these tasks.</span></span>

<span data-ttu-id="e0428-126">**Per creare il database di servizi di applicazione**</span><span class="sxs-lookup"><span data-stu-id="e0428-126">**To create the application services database**</span></span>

1. <span data-ttu-id="e0428-127">Aprire il prompt dei comandi di Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="e0428-127">Open a Visual Studio 2010 command prompt.</span></span> <span data-ttu-id="e0428-128">A tale scopo, scegliere il **avviare** dal menu **tutti i programmi**, fare clic su **Microsoft Visual Studio 2010**, fare clic su **Visual Studio Tools**e quindi Fare clic su **prompt dei comandi di Visual Studio (2010)**.</span><span class="sxs-lookup"><span data-stu-id="e0428-128">To do this, on the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, click **Visual Studio Tools**, and then click **Visual Studio Command Prompt (2010)**.</span></span>
2. <span data-ttu-id="e0428-129">Al prompt dei comandi, digitare il comando seguente e quindi premere INVIO:</span><span class="sxs-lookup"><span data-stu-id="e0428-129">At the command prompt, type this command, and then press Enter:</span></span>

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. <span data-ttu-id="e0428-130">Usare la **– C** per specificare la stringa di connessione per il server di database.</span><span class="sxs-lookup"><span data-stu-id="e0428-130">Use the **–C** switch to specify the connection string for your database server.</span></span>
    2. <span data-ttu-id="e0428-131">Usare la **– A** per specificare funzionalità da aggiungere al database dei servizi dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e0428-131">Use the **–A** switch to specify the application services features you want to add to the database.</span></span> <span data-ttu-id="e0428-132">In questo caso **m** indica che si desidera aggiungere il supporto per il provider di appartenenze e **r** indica che si desidera aggiungere il supporto per la gestione dei ruoli.</span><span class="sxs-lookup"><span data-stu-id="e0428-132">In this case, **m** indicates that you want to add support for the membership provider and **r** indicates that you want to add support for the role manager.</span></span>
    3. <span data-ttu-id="e0428-133">Usare la **– d** per specificare un nome per il database di servizi di applicazione.</span><span class="sxs-lookup"><span data-stu-id="e0428-133">Use the **–d** switch to specify a name for your application services database.</span></span> <span data-ttu-id="e0428-134">Se si omette questa opzione è attivata, l'utilità creerà un database con il nome predefinito della **aspnetdb**.</span><span class="sxs-lookup"><span data-stu-id="e0428-134">If you omit this switch, the utility will create a database with the default name of **aspnetdb**.</span></span>
3. <span data-ttu-id="e0428-135">Quando il database è stato creato correttamente, il prompt dei comandi visualizzerà un messaggio di conferma.</span><span class="sxs-lookup"><span data-stu-id="e0428-135">When the database has been created successfully, the command prompt will show a confirmation.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="e0428-136">Per altre informazioni su aspnet\_regsql utilità, vedere [strumento di registrazione di SQL Server ASP.NET (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="e0428-136">For more information on the aspnet\_regsql utility, see [ASP.NET SQL Server Registration Tool (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).</span></span>


<span data-ttu-id="e0428-137">Il passaggio successivo è assicurarsi che le stringhe di connessione della soluzione Contact Manager puntano all'istanza locale di SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="e0428-137">The next step is to make sure that the connection strings in the Contact Manager solution point to your local instance of SQL Server Express.</span></span>

<span data-ttu-id="e0428-138">**Per aggiornare le stringhe di connessione**</span><span class="sxs-lookup"><span data-stu-id="e0428-138">**To update the connection strings**</span></span>

1. <span data-ttu-id="e0428-139">Aprire la soluzione Contact Manager in Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="e0428-139">Open the Contact Manager solution in Visual Studio 2010.</span></span>
2. <span data-ttu-id="e0428-140">Nel **Esplora soluzioni** finestra, espandere il **ContactManager.Mvc** del progetto e quindi fare doppio clic il **Web. config** nodo.</span><span class="sxs-lookup"><span data-stu-id="e0428-140">In the **Solution Explorer** window, expand the **ContactManager.Mvc** project, and then double-click the **Web.config** node.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e0428-141">Il progetto ContactManager.Mvc include due *Web. config* file.</span><span class="sxs-lookup"><span data-stu-id="e0428-141">The ContactManager.Mvc project includes two *web.config* files.</span></span> <span data-ttu-id="e0428-142">È necessario modificare il file a livello di progetto.</span><span class="sxs-lookup"><span data-stu-id="e0428-142">You need to edit the project-level file.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. <span data-ttu-id="e0428-143">Nel **connectionStrings** elemento, verificare che la stringa di connessione denominata **ApplicationServices** punti al database servizi dell'applicazione ASP.NET locale.</span><span class="sxs-lookup"><span data-stu-id="e0428-143">In the **connectionStrings** element, verify that the connection string named **ApplicationServices** points to your local ASP.NET application services database.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. <span data-ttu-id="e0428-144">Nel **Esplora soluzioni** finestra, espandere il **ContactManager.Service** del progetto e quindi fare doppio clic il **Web. config** nodo.</span><span class="sxs-lookup"><span data-stu-id="e0428-144">In the **Solution Explorer** window, expand the **ContactManager.Service** project, and then double-click the **Web.config** node.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. <span data-ttu-id="e0428-145">Nel **connectionStrings** elemento, nella stringa di connessione denominata **ContactManagerContext**, verificare che il **Zdroj dat** viene impostata per l'istanza locale di SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="e0428-145">In the **connectionStrings** element, in the connection string named **ContactManagerContext**, verify that the **Data Source** property is set to your local instance of SQL Server Express.</span></span> <span data-ttu-id="e0428-146">Non devi si modifica nient'altro nella stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="e0428-146">You don't need to change anything else in the connection string.</span></span>

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. <span data-ttu-id="e0428-147">Salvare tutti i file aperti.</span><span class="sxs-lookup"><span data-stu-id="e0428-147">Save all open files.</span></span>

<span data-ttu-id="e0428-148">A questo punto dovrebbe essere pronto per l'esecuzione della soluzione Contact Manager nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="e0428-148">You should now be ready to run the Contact Manager solution on your local machine.</span></span>

> [!NOTE]
> <span data-ttu-id="e0428-149">Se si seguono questi passaggi senza aver prima creato un database di servizi dell'applicazione, ASP.NET verrà creato il database la prima volta che si tenta di creare un utente.</span><span class="sxs-lookup"><span data-stu-id="e0428-149">If you follow these steps without first creating an application services database, ASP.NET will create the database the first time you attempt to create a user.</span></span> <span data-ttu-id="e0428-150">Tuttavia, creazione manuale del database offre molto più controllo all'interno del set di funzionalità di servizi dell'applicazione che si desidera supportare.</span><span class="sxs-lookup"><span data-stu-id="e0428-150">However, manually creating the database gives you a lot more control over the application services feature set you want to support.</span></span>


<span data-ttu-id="e0428-151">**Per eseguire la soluzione Contact Manager**</span><span class="sxs-lookup"><span data-stu-id="e0428-151">**To run the Contact Manager solution**</span></span>

1. <span data-ttu-id="e0428-152">In Visual Studio 2010, premere F5.</span><span class="sxs-lookup"><span data-stu-id="e0428-152">In Visual Studio 2010, press F5.</span></span>
2. <span data-ttu-id="e0428-153">Internet Explorer viene avviato e richiede l'URL dell'applicazione Contact Manager ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="e0428-153">Internet Explorer starts up and requests the URL of the Contact Manager ASP.NET MVC 3 application.</span></span> <span data-ttu-id="e0428-154">Per impostazione predefinita, l'applicazione viene visualizzata la **tutti i contatti** pagina.</span><span class="sxs-lookup"><span data-stu-id="e0428-154">By default, the application displays the **All Contacts** page.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. <span data-ttu-id="e0428-155">Aggiungere alcuni contatti e quindi verificare che l'applicazione funzioni come previsto.</span><span class="sxs-lookup"><span data-stu-id="e0428-155">Add a few contacts, and then verify that the application works as expected.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. <span data-ttu-id="e0428-156">Passare a `http://localhost:50114/Account/Register` (se si ospita l'applicazione su una porta diversa, modificare l'URL).</span><span class="sxs-lookup"><span data-stu-id="e0428-156">Browse to `http://localhost:50114/Account/Register` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="e0428-157">Aggiungere un nome utente, indirizzo di posta elettronica e password e verificare che tu sia in grado di registrare correttamente un account.</span><span class="sxs-lookup"><span data-stu-id="e0428-157">Add a user name, email address, and password, and verify that you're able to register an account successfully.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. <span data-ttu-id="e0428-158">Passare a `http://localhost:50114/Account/LogOn` (se si ospita l'applicazione su una porta diversa, modificare l'URL).</span><span class="sxs-lookup"><span data-stu-id="e0428-158">Browse to `http://localhost:50114/Account/LogOn` (adjust the URL if you're hosting the application on a different port).</span></span> <span data-ttu-id="e0428-159">Verificare che tu sia in grado di accedere usando l'account che appena creato.</span><span class="sxs-lookup"><span data-stu-id="e0428-159">Verify that you're able to log on using the account you just created.</span></span>

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. <span data-ttu-id="e0428-160">Chiudere Internet Explorer per arrestare il debug.</span><span class="sxs-lookup"><span data-stu-id="e0428-160">Close Internet Explorer to stop debugging.</span></span>

## <a name="conclusion"></a><span data-ttu-id="e0428-161">Conclusione</span><span class="sxs-lookup"><span data-stu-id="e0428-161">Conclusion</span></span>

<span data-ttu-id="e0428-162">A questo punto, la soluzione Contact Manager deve essere completamente configurata per l'esecuzione nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="e0428-162">At this point, the Contact Manager solution should be fully configured to run on your local machine.</span></span> <span data-ttu-id="e0428-163">Quando si utilizzano gli altri argomenti in questa esercitazione, è possibile usare la soluzione come riferimento.</span><span class="sxs-lookup"><span data-stu-id="e0428-163">You can use the solution as a reference when you work through the other topics in this tutorial.</span></span>

<span data-ttu-id="e0428-164">Argomento successivo [informazioni sul File di progetto](understanding-the-project-file.md), illustra come è possibile usare i file di progetto personalizzati di Microsoft Build Engine (MSBuild) all'interno della soluzione Contact Manager per controllare il processo di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="e0428-164">The next topic, [Understanding the Project File](understanding-the-project-file.md), explains how you can use the custom Microsoft Build Engine (MSBuild) project files within the Contact Manager solution to control the deployment process.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e0428-165">[Precedente](the-contact-manager-solution.md)
> [Successivo](understanding-the-project-file.md)</span><span class="sxs-lookup"><span data-stu-id="e0428-165">[Previous](the-contact-manager-solution.md)
[Next](understanding-the-project-file.md)</span></span>
