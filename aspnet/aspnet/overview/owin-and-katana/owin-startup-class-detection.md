---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Rilevamento di classe di avvio OWIN | Documenti Microsoft
author: Praburaj
description: In questa esercitazione viene illustrato come configurare la classe di avvio OWIN viene caricata. Per ulteriori informazioni su OWIN, vedere una panoramica del progetto Katana. In questa esercitazione è stata...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: 33d2745b24387419e5614c62c2d46948427b242a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="owin-startup-class-detection"></a><span data-ttu-id="88cb9-105">Rilevamento di classe avvio OWIN</span><span class="sxs-lookup"><span data-stu-id="88cb9-105">OWIN Startup Class Detection</span></span>
====================
<span data-ttu-id="88cb9-106">dal [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="88cb9-106">by [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="88cb9-107">In questa esercitazione viene illustrato come configurare la classe di avvio OWIN viene caricata.</span><span class="sxs-lookup"><span data-stu-id="88cb9-107">This tutorial shows how to configure which OWIN startup class is loaded.</span></span> <span data-ttu-id="88cb9-108">Per ulteriori informazioni su OWIN, vedere [una panoramica del progetto Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="88cb9-108">For more information on OWIN, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="88cb9-109">In questa esercitazione è stato scritto dal Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan e Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).</span><span class="sxs-lookup"><span data-stu-id="88cb9-109">This tutorial was written by Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan, and Howard Dierking ( [@howard\_dierking](https://twitter.com/howard_dierking) ).</span></span>
> 
> ## <a name="prerequisites"></a><span data-ttu-id="88cb9-110">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="88cb9-110">Prerequisites</span></span>
> 
> [<span data-ttu-id="88cb9-111">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="88cb9-111">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)


## <a name="owin-startup-class-detection"></a><span data-ttu-id="88cb9-112">Rilevamento di classe avvio OWIN</span><span class="sxs-lookup"><span data-stu-id="88cb9-112">OWIN Startup Class Detection</span></span>

 <span data-ttu-id="88cb9-113">Ogni applicazione OWIN dispone di una classe di avvio in cui è necessario specificare i componenti per la pipeline dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="88cb9-113">Every OWIN Application has a startup class where you specify components for the application pipeline.</span></span> <span data-ttu-id="88cb9-114">Esistono diversi modi, è possibile collegare la classe di avvio con il runtime, a seconda del modello di hosting è scegliere (OwinHost, IIS e IIS Express).</span><span class="sxs-lookup"><span data-stu-id="88cb9-114">There are different ways you can connect your startup class with the runtime, depending on the hosting model you choose (OwinHost, IIS, and IIS-Express).</span></span> <span data-ttu-id="88cb9-115">La classe di avvio illustrata in questa esercitazione è utilizzabile in ogni applicazione di hosting.</span><span class="sxs-lookup"><span data-stu-id="88cb9-115">The startup class shown in this tutorial can be used in every hosting application.</span></span> <span data-ttu-id="88cb9-116">Ci si connette la classe di avvio con il runtime hosting usando uno di questi approcci:</span><span class="sxs-lookup"><span data-stu-id="88cb9-116">You connect the startup class with the hosting runtime using one of the these approaches:</span></span>  

1. <span data-ttu-id="88cb9-117">**Convenzione di denominazione**: esegue la ricerca di una classe denominata Katana `Startup` nello spazio dei nomi corrispondenti al nome di assembly o lo spazio dei nomi globale.</span><span class="sxs-lookup"><span data-stu-id="88cb9-117">**Naming Convention**: Katana looks for a class named `Startup` in namespace matching the assembly name or the global namespace.</span></span>
2. <span data-ttu-id="88cb9-118">**Attributo OwinStartup**: si tratta dell'approccio richiederà maggior parte degli sviluppatori per specificare la classe di avvio.</span><span class="sxs-lookup"><span data-stu-id="88cb9-118">**OwinStartup Attribute**: This is the approach most developers will take to specify the startup class.</span></span> <span data-ttu-id="88cb9-119">L'attributo seguente verrà impostata la classe di avvio di `TestStartup` classe il `StartupDemo` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="88cb9-119">The following attribute will set the startup class to the `TestStartup` class in the `StartupDemo` namespace.</span></span> 

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   <span data-ttu-id="88cb9-120">Il `OwinStartup` attributo prevarrà sulla convenzione di denominazione.</span><span class="sxs-lookup"><span data-stu-id="88cb9-120">The `OwinStartup` attribute overrides the naming convention.</span></span> <span data-ttu-id="88cb9-121">È inoltre possibile specificare un nome descrittivo con questo attributo, tuttavia, utilizzando un nome descrittivo è necessario utilizzare anche il `appSetting` elemento nel file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="88cb9-121">You can also specify a friendly name with this attribute, however, using a friendly name requires you to also use the `appSetting` element in the configuration file.</span></span>
3. <span data-ttu-id="88cb9-122">**L'elemento appSetting nel file di configurazione**: il `appSetting` elemento esegue l'override di `OwinStartup` attributo e convenzione di denominazione.</span><span class="sxs-lookup"><span data-stu-id="88cb9-122">**The appSetting element in the Configuration file**: The `appSetting` element overrides the `OwinStartup` attribute and naming convention.</span></span> <span data-ttu-id="88cb9-123">È possibile avere più classi di avvio (ogni usando un `OwinStartup` attributo) e configurare la classe di avvio verrà caricata in un file di configurazione utilizzando codice simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="88cb9-123">You can have multiple startup classes (each using an `OwinStartup` attribute) and configure which startup class will be loaded in a configuration file using markup similar to the following:</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   <span data-ttu-id="88cb9-124">Consente inoltre la chiave seguente, che specifica in modo esplicito la classe di avvio e l'assembly:</span><span class="sxs-lookup"><span data-stu-id="88cb9-124">The following key, which explicitly specifies the startup class and assembly can also be used:</span></span> 

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   <span data-ttu-id="88cb9-125">Il seguente codice XML nel file di configurazione specifica un nome della classe di avvio descrittivo `ProductionConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="88cb9-125">The following XML in the configuration file specifies a friendly startup class name of `ProductionConfiguration`.</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   <span data-ttu-id="88cb9-126">Il codice precedente deve essere utilizzato con le operazioni seguenti `OwinStartup` attributo che specifica un nome descrittivo e fa sì che la `ProductionStartup2` classe per l'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="88cb9-126">The above markup must be used with the following `OwinStartup` attribute which specifies a friendly name and causes the `ProductionStartup2` class to run.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. <span data-ttu-id="88cb9-127">Per disabilitare il rilevamento di avvio OWIN aggiungere il `appSetting owin:AutomaticAppStartup` con un valore di `"false"` nel file Web. config.</span><span class="sxs-lookup"><span data-stu-id="88cb9-127">To disable OWIN startup discovery add the `appSetting owin:AutomaticAppStartup` with a value of `"false"` in the web.config file.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a><span data-ttu-id="88cb9-128">Creare un'App Web ASP.NET tramite l'avvio di OWIN</span><span class="sxs-lookup"><span data-stu-id="88cb9-128">Create an ASP.NET Web App using OWIN Startup</span></span>

1. <span data-ttu-id="88cb9-129">Creare un'applicazione web Asp.Net vuota e denominarla **StartupDemo**.</span><span class="sxs-lookup"><span data-stu-id="88cb9-129">Create an empty Asp.Net web application and name it **StartupDemo**.</span></span> <span data-ttu-id="88cb9-130">-Installare `Microsoft.Owin.Host.SystemWeb` tramite Gestione pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="88cb9-130">- Install `Microsoft.Owin.Host.SystemWeb` using the NuGet package manager.</span></span> <span data-ttu-id="88cb9-131">Dal **strumenti** dal menu **Gestione pacchetti libreria**e quindi **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="88cb9-131">From the **Tools** menu, select **Library Package Manager**, and then **Package Manager Console**.</span></span> <span data-ttu-id="88cb9-132">Immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="88cb9-132">Enter the following command:</span></span>  

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. <span data-ttu-id="88cb9-133">Aggiungere una classe di avvio OWIN.</span><span class="sxs-lookup"><span data-stu-id="88cb9-133">Add an OWIN startup class.</span></span> <span data-ttu-id="88cb9-134">In Visual Studio 2013 destro fare clic sul progetto e selezionare **Aggiungi classe**. - In di **Aggiungi nuovo elemento** finestra di dialogo immettere *OWIN* nel campo di ricerca e modificare il nome in Startup.cs, e quindi fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="88cb9-134">In Visual Studio 2013 right click the project and select **Add Class**.- In the **Add New Item** dialog box, enter *OWIN* in the search field, and change the name to Startup.cs, and then click **Add**.</span></span>  
  
     ![](owin-startup-class-detection/_static/image1.png)   
  
   <span data-ttu-id="88cb9-135">Alla successiva che si desidera aggiungere un *classe di avvio di Owin*, sarà disponibile il **Aggiungi** menu.</span><span class="sxs-lookup"><span data-stu-id="88cb9-135">The next time you want to add an *Owin Startup class*, it will be in available from the **Add** menu.</span></span>  
   
     ![](owin-startup-class-detection/_static/image2.png)  
  
   <span data-ttu-id="88cb9-136">In alternativa, è possibile fare clic con il pulsante destro del progetto e selezionare **Aggiungi**, quindi selezionare **nuovo elemento**, quindi selezionare il **classe di avvio di Owin**.</span><span class="sxs-lookup"><span data-stu-id="88cb9-136">Alternatively, you can right click the project and select **Add**, then select **New Item**, and then select the **Owin Startup class**.</span></span>  
  
     ![](owin-startup-class-detection/_static/image3.png)  
  
- <span data-ttu-id="88cb9-137">Sostituire il codice generato nei componenti di *Startup.cs* file con le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="88cb9-137">Replace the generated code in the *Startup.cs* file with the following:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]
  
  <span data-ttu-id="88cb9-138">Il `app.Use` espressione lambda viene utilizzata per registrare il componente specificato middleware alla pipeline OWIN.</span><span class="sxs-lookup"><span data-stu-id="88cb9-138">The `app.Use` lambda expression is used to register the specified middleware component to the OWIN pipeline.</span></span> <span data-ttu-id="88cb9-139">In questo caso viene impostata la registrazione delle richieste in ingresso prima di rispondere alla richiesta in ingresso.</span><span class="sxs-lookup"><span data-stu-id="88cb9-139">In this case we are setting up logging of incoming requests before responding to the incoming request.</span></span> <span data-ttu-id="88cb9-140">Il `next` parametro è il delegato ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [attività](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) al componente successivo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="88cb9-140">The `next` parameter is the delegate ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) to the next component in the pipeline.</span></span> <span data-ttu-id="88cb9-141">Il `app.Run` espressione lambda associa la pipeline di richieste in ingresso e fornisce il meccanismo di risposta.</span><span class="sxs-lookup"><span data-stu-id="88cb9-141">The `app.Run` lambda expression hooks up the pipeline to incoming requests and provides the response mechanism.</span></span>
     > [!NOTE]
     > <span data-ttu-id="88cb9-142">Nel codice precedente è stata impostata come commento il `OwinStartup` attributo e ci stiamo affidarsi la convenzione di in esecuzione la classe denominata `Startup` .-premere ***F5*** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="88cb9-142">In the code above we have commented out the `OwinStartup` attribute and we're relying on the convention of running the class named `Startup` .- Press ***F5*** to run the application.</span></span> <span data-ttu-id="88cb9-143">Fare clic su Aggiorna più volte.</span><span class="sxs-lookup"><span data-stu-id="88cb9-143">Hit refresh a few times.</span></span>  
  
    ![](owin-startup-class-detection/_static/image4.png)  
  <span data-ttu-id="88cb9-144">Nota: Il numero indicato tra le immagini in questa esercitazione non corrisponderà al numero viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="88cb9-144">Note: The number shown in the images in this tutorial will not match the number you see.</span></span> <span data-ttu-id="88cb9-145">La stringa di millisecondo viene utilizzata per visualizzare una nuova risposta quando si aggiorna la pagina.</span><span class="sxs-lookup"><span data-stu-id="88cb9-145">The millisecond string is used to show a new response when you refresh the page.</span></span>  
  <span data-ttu-id="88cb9-146">È possibile visualizzare le informazioni della traccia di **Output** finestra.</span><span class="sxs-lookup"><span data-stu-id="88cb9-146">You can see the trace information in the **Output** window.</span></span>  
  
    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a><span data-ttu-id="88cb9-147">Aggiungere ulteriori classi di avvio</span><span class="sxs-lookup"><span data-stu-id="88cb9-147">Add More Startup Classes</span></span>

<span data-ttu-id="88cb9-148">In questa sezione si aggiungerà un'altra classe di avvio.</span><span class="sxs-lookup"><span data-stu-id="88cb9-148">In this section we'll add another Startup class.</span></span> <span data-ttu-id="88cb9-149">È possibile aggiungere più classe di avvio OWIN per l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="88cb9-149">You can add multiple OWIN startup class to your application.</span></span> <span data-ttu-id="88cb9-150">Potrebbe ad esempio, si desidera creare classi di avvio per lo sviluppo, test e produzione.</span><span class="sxs-lookup"><span data-stu-id="88cb9-150">For example, you might want to create startup classes for development, testing and production.</span></span>

1. <span data-ttu-id="88cb9-151">Creare una nuova classe di avvio di OWIN e denominarla `ProductionStartup`.</span><span class="sxs-lookup"><span data-stu-id="88cb9-151">Create a new OWIN Startup class and name it `ProductionStartup`.</span></span>
2. <span data-ttu-id="88cb9-152">Sostituire il codice generato con il seguente:</span><span class="sxs-lookup"><span data-stu-id="88cb9-152">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. <span data-ttu-id="88cb9-153">Premere CTRL F5 per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="88cb9-153">Press Control F5 to run the app.</span></span> <span data-ttu-id="88cb9-154">Il `OwinStartup` attributo specifica la classe di avvio di produzione viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="88cb9-154">The `OwinStartup` attribute specifies the production startup class is run.</span></span>  
  
    ![](owin-startup-class-detection/_static/image6.png)
4. <span data-ttu-id="88cb9-155">Creare un'altra classe di avvio di OWIN e denominarla `TestStartup`.</span><span class="sxs-lookup"><span data-stu-id="88cb9-155">Create another OWIN Startup class and name it `TestStartup`.</span></span>
5. <span data-ttu-id="88cb9-156">Sostituire il codice generato con il seguente:</span><span class="sxs-lookup"><span data-stu-id="88cb9-156">Replace the generated code with the following:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   <span data-ttu-id="88cb9-157">Il `OwinStartup` overload dell'attributo precedente specifica `TestingConfiguration` come il *descrittivo* nome della classe di avvio.</span><span class="sxs-lookup"><span data-stu-id="88cb9-157">The `OwinStartup` attribute overload above specifies `TestingConfiguration` as the *friendly* name of the Startup class.</span></span>
6. <span data-ttu-id="88cb9-158">Aprire il *Web. config* file e aggiungere la chiave di avvio di OWIN App che specifica il nome descrittivo della classe di avvio:</span><span class="sxs-lookup"><span data-stu-id="88cb9-158">Open the *web.config* file and add the OWIN App startup key which specifies the friendly name of the Startup class:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. <span data-ttu-id="88cb9-159">Premere CTRL F5 per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="88cb9-159">Press Control F5 to run the app.</span></span> <span data-ttu-id="88cb9-160">L'elemento impostazioni app accetta precedenti e il test di configurazione viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="88cb9-160">The app settings element takes precedent, and the test configuration is run.</span></span>  
  
    ![](owin-startup-class-detection/_static/image7.png)
8. <span data-ttu-id="88cb9-161">Rimuovere il *descrittivo* nome dal `OwinStartup` attributo la `TestStartup` classe.</span><span class="sxs-lookup"><span data-stu-id="88cb9-161">Remove the *friendly* name from the `OwinStartup` attribute in the `TestStartup` class.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. <span data-ttu-id="88cb9-162">Sostituire la chiave di avvio applicazione OWIN il *Web. config* file con le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="88cb9-162">Replace the OWIN App startup key in the *web.config* file with the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. <span data-ttu-id="88cb9-163">Ripristinare il `OwinStartup` attributo in ogni classe per il codice di attributo predefinito generato da Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="88cb9-163">Revert the `OwinStartup` attribute in each class to the default attribute code generated by Visual Studio:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    <span data-ttu-id="88cb9-164">Tutte le chiavi di avvio di OWIN App seguente della causerà l'esecuzione della classe di produzione.</span><span class="sxs-lookup"><span data-stu-id="88cb9-164">Each of the OWIN App startup keys below will cause the production class to run.</span></span> 

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    <span data-ttu-id="88cb9-165">La chiave di avvio ultima specifica il metodo di configurazione di avvio.</span><span class="sxs-lookup"><span data-stu-id="88cb9-165">The last startup key specifies the startup configuration method.</span></span> <span data-ttu-id="88cb9-166">La seguente chiave di avvio di OWIN App consente di modificare il nome della classe di configurazione per `MyConfiguration` .</span><span class="sxs-lookup"><span data-stu-id="88cb9-166">The following OWIN App startup key allows you to change the name of the configuration class to `MyConfiguration` .</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a><span data-ttu-id="88cb9-167">Utilizzando Owinhost.exe</span><span class="sxs-lookup"><span data-stu-id="88cb9-167">Using Owinhost.exe</span></span>

1. <span data-ttu-id="88cb9-168">Sostituire il file Web. config con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="88cb9-168">Replace the Web.config file with the following markup:</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   <span data-ttu-id="88cb9-169">L'ultima chiave wins, pertanto in questo caso `TestStartup` specificato.</span><span class="sxs-lookup"><span data-stu-id="88cb9-169">The last key wins, so in this case `TestStartup` is specified.</span></span>
2. <span data-ttu-id="88cb9-170">Installare Owinhost da PMC:</span><span class="sxs-lookup"><span data-stu-id="88cb9-170">Install Owinhost from the PMC:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. <span data-ttu-id="88cb9-171">Passare alla cartella dell'applicazione (la cartella contenente il *Web. config* file) e in un prompt dei comandi e tipo:</span><span class="sxs-lookup"><span data-stu-id="88cb9-171">Navigate to the application folder (the folder containing the *Web.config* file) and in a command prompt and type:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   <span data-ttu-id="88cb9-172">Verrà visualizzata la finestra di comando:</span><span class="sxs-lookup"><span data-stu-id="88cb9-172">The command window will show:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. <span data-ttu-id="88cb9-173">Avviare un browser con URL `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="88cb9-173">Launch a browser with the URL `http://localhost:5000/`.</span></span>  
  
    ![](owin-startup-class-detection/_static/image8.png)  
  
   <span data-ttu-id="88cb9-174">OwinHost rispettate le convenzioni di avvio elencate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="88cb9-174">OwinHost honored the startup conventions listed above.</span></span>
5. <span data-ttu-id="88cb9-175">Nella finestra di comando, premere INVIO per uscire dall'installazione OwinHost.</span><span class="sxs-lookup"><span data-stu-id="88cb9-175">In the command window, press Enter to exit OwinHost.</span></span>
6. <span data-ttu-id="88cb9-176">Nel `ProductionStartup` classe, aggiungere l'attributo OwinStartup seguente che specifica un nome descrittivo del *ProductionConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="88cb9-176">In the `ProductionStartup` class, add the following OwinStartup attribute which specifies a friendly name of *ProductionConfiguration*.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. <span data-ttu-id="88cb9-177">Nel prompt dei comandi e digitare:</span><span class="sxs-lookup"><span data-stu-id="88cb9-177">In the command prompt and type:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   <span data-ttu-id="88cb9-178">Viene caricata la classe di avvio di produzione.</span><span class="sxs-lookup"><span data-stu-id="88cb9-178">The Production startup class is loaded.</span></span>  
    ![](owin-startup-class-detection/_static/image9.png)  
   <span data-ttu-id="88cb9-179">L'applicazione dispone di più classi di avvio e in questo esempio è stata posticipata le classi di avvio da caricare fino al runtime.</span><span class="sxs-lookup"><span data-stu-id="88cb9-179">Our application has multiple startup classes, and in this example we have deferred which startup class to load until runtime.</span></span>
8. <span data-ttu-id="88cb9-180">Verificare le seguenti opzioni di avvio di runtime:</span><span class="sxs-lookup"><span data-stu-id="88cb9-180">Test the following runtime startup options:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
