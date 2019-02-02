---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Rilevamento della classe di avvio OWIN | Microsoft Docs
author: Praburaj
description: Questa esercitazione illustra come configurare la classe di avvio OWIN viene caricata. Per altre informazioni su OWIN, vedere una panoramica del progetto Katana. Questa esercitazione è stata...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: 0b34cca8b48383dbb028106651758dff889ed614
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667297"
---
<a name="owin-startup-class-detection"></a><span data-ttu-id="4b59d-105">Rilevamento della classe di avvio OWIN</span><span class="sxs-lookup"><span data-stu-id="4b59d-105">OWIN Startup Class Detection</span></span>
====================

> <span data-ttu-id="4b59d-106">Questa esercitazione illustra come configurare la classe di avvio OWIN viene caricata.</span><span class="sxs-lookup"><span data-stu-id="4b59d-106">This tutorial shows how to configure which OWIN startup class is loaded.</span></span> <span data-ttu-id="4b59d-107">Per altre informazioni su OWIN, vedere [una panoramica del progetto Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="4b59d-107">For more information on OWIN, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="4b59d-108">Questa esercitazione è stato scritto da Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan così Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).</span><span class="sxs-lookup"><span data-stu-id="4b59d-108">This tutorial was written by Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan, and Howard Dierking ( [@howard\_dierking](https://twitter.com/howard_dierking) ).</span></span>
>
> ## <a name="prerequisites"></a><span data-ttu-id="4b59d-109">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="4b59d-109">Prerequisites</span></span>
>
> [<span data-ttu-id="4b59d-110">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="4b59d-110">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/)


## <a name="owin-startup-class-detection"></a><span data-ttu-id="4b59d-111">Rilevamento della classe di avvio OWIN</span><span class="sxs-lookup"><span data-stu-id="4b59d-111">OWIN Startup Class Detection</span></span>

 <span data-ttu-id="4b59d-112">Ogni applicazione OWIN dispone di una classe di avvio in cui è specificare i componenti della pipeline dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4b59d-112">Every OWIN Application has a startup class where you specify components for the application pipeline.</span></span> <span data-ttu-id="4b59d-113">Esistono diversi modi, è possibile connettere la classe di avvio con il runtime, a seconda del modello di hosting scelto (OwinHost, IIS e IIS Express).</span><span class="sxs-lookup"><span data-stu-id="4b59d-113">There are different ways you can connect your startup class with the runtime, depending on the hosting model you choose (OwinHost, IIS, and IIS-Express).</span></span> <span data-ttu-id="4b59d-114">La classe di avvio illustrata in questa esercitazione può essere utilizzata in ogni applicazione di hosting.</span><span class="sxs-lookup"><span data-stu-id="4b59d-114">The startup class shown in this tutorial can be used in every hosting application.</span></span> <span data-ttu-id="4b59d-115">La classe startup è connettersi con il runtime hosting usando uno di questi approcci:</span><span class="sxs-lookup"><span data-stu-id="4b59d-115">You connect the startup class with the hosting runtime using one of the these approaches:</span></span>

1. <span data-ttu-id="4b59d-116">**Convenzione di denominazione**: Esegue la ricerca di una classe denominata Katana `Startup` nello spazio dei nomi corrispondenti al nome dell'assembly o lo spazio dei nomi globale.</span><span class="sxs-lookup"><span data-stu-id="4b59d-116">**Naming Convention**: Katana looks for a class named `Startup` in namespace matching the assembly name or the global namespace.</span></span>
2. <span data-ttu-id="4b59d-117">**Attributo OwinStartup**: Questo è l'approccio che richiederà maggior parte degli sviluppatori per specificare la classe di avvio.</span><span class="sxs-lookup"><span data-stu-id="4b59d-117">**OwinStartup Attribute**: This is the approach most developers will take to specify the startup class.</span></span> <span data-ttu-id="4b59d-118">L'attributo seguente imposterà classe di avvio il `TestStartup` classe la `StartupDemo` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="4b59d-118">The following attribute will set the startup class to the `TestStartup` class in the `StartupDemo` namespace.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   <span data-ttu-id="4b59d-119">Il `OwinStartup` attributo sostituisce la convenzione di denominazione.</span><span class="sxs-lookup"><span data-stu-id="4b59d-119">The `OwinStartup` attribute overrides the naming convention.</span></span> <span data-ttu-id="4b59d-120">È anche possibile specificare un nome descrittivo con questo attributo, tuttavia, utilizzando un nome descrittivo richiede che venga utilizzato anche il `appSetting` elemento nel file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="4b59d-120">You can also specify a friendly name with this attribute, however, using a friendly name requires you to also use the `appSetting` element in the configuration file.</span></span>
3. <span data-ttu-id="4b59d-121">**L'elemento appSetting nel file di configurazione**: Il `appSetting` esegue l'override di elemento di `OwinStartup` attributo e la convenzione di denominazione.</span><span class="sxs-lookup"><span data-stu-id="4b59d-121">**The appSetting element in the Configuration file**: The `appSetting` element overrides the `OwinStartup` attribute and naming convention.</span></span> <span data-ttu-id="4b59d-122">È possibile avere più classi di avvio (ogni usando un `OwinStartup` attributo) e configurare la classe di avvio verrà caricata in un file di configurazione tramite markup simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="4b59d-122">You can have multiple startup classes (each using an `OwinStartup` attribute) and configure which startup class will be loaded in a configuration file using markup similar to the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   <span data-ttu-id="4b59d-123">Consente inoltre la chiave seguente, che specifica in modo esplicito la classe di avvio e l'assembly:</span><span class="sxs-lookup"><span data-stu-id="4b59d-123">The following key, which explicitly specifies the startup class and assembly can also be used:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   <span data-ttu-id="4b59d-124">Il codice XML seguente nel file di configurazione specifica un nome della classe di avvio descrittivo `ProductionConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="4b59d-124">The following XML in the configuration file specifies a friendly startup class name of `ProductionConfiguration`.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   <span data-ttu-id="4b59d-125">Il markup riportato sopra deve essere usato con quanto segue `OwinStartup` attributo che specifica un nome descrittivo e fa sì che il `ProductionStartup2` classe per l'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="4b59d-125">The above markup must be used with the following `OwinStartup` attribute which specifies a friendly name and causes the `ProductionStartup2` class to run.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. <span data-ttu-id="4b59d-126">Per disabilitare il rilevamento di avvio OWIN, aggiungere il `appSetting owin:AutomaticAppStartup` con il valore `"false"` nel file Web. config.</span><span class="sxs-lookup"><span data-stu-id="4b59d-126">To disable OWIN startup discovery add the `appSetting owin:AutomaticAppStartup` with a value of `"false"` in the web.config file.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a><span data-ttu-id="4b59d-127">Creare un'App Web ASP.NET con OWIN Startup</span><span class="sxs-lookup"><span data-stu-id="4b59d-127">Create an ASP.NET Web App using OWIN Startup</span></span>

1. <span data-ttu-id="4b59d-128">Creare un'applicazione web Asp.Net vuota e denominarla **StartupDemo**.</span><span class="sxs-lookup"><span data-stu-id="4b59d-128">Create an empty Asp.Net web application and name it **StartupDemo**.</span></span> <span data-ttu-id="4b59d-129">-Installare `Microsoft.Owin.Host.SystemWeb` usando Gestione pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="4b59d-129">- Install `Microsoft.Owin.Host.SystemWeb` using the NuGet package manager.</span></span> <span data-ttu-id="4b59d-130">Dal **strumenti** dal menu **Gestione pacchetti NuGet**, quindi **la Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="4b59d-130">From the **Tools** menu, select **NuGet Package Manager**, and then **Package Manager Console**.</span></span> <span data-ttu-id="4b59d-131">Immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="4b59d-131">Enter the following command:</span></span>

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. <span data-ttu-id="4b59d-132">Aggiungere una classe di avvio OWIN.</span><span class="sxs-lookup"><span data-stu-id="4b59d-132">Add an OWIN startup class.</span></span> <span data-ttu-id="4b59d-133">In Visual Studio 2017 fare clic sul progetto e scegliere **Aggiungi classe**. - nel **Aggiungi nuovo elemento** finestra di dialogo immettere *OWIN* nel campo di ricerca e modificare il nome in Startup.cs, e quindi selezionare **Add**.</span><span class="sxs-lookup"><span data-stu-id="4b59d-133">In Visual Studio 2017 right-click the project and select **Add Class**.- In the **Add New Item** dialog box, enter *OWIN* in the search field, and change the name to Startup.cs, and then select **Add**.</span></span>

     ![](owin-startup-class-detection/_static/image1.png)

   <span data-ttu-id="4b59d-134">La volta successiva che si desidera aggiungere un *Owin Startup class*, sarà disponibile dal **Add** menu.</span><span class="sxs-lookup"><span data-stu-id="4b59d-134">The next time you want to add an *Owin Startup class*, it will be in available from the **Add** menu.</span></span>

     ![](owin-startup-class-detection/_static/image2.png)

   <span data-ttu-id="4b59d-135">In alternativa, è possibile fare clic sul progetto e selezionare **Add**, quindi selezionare **nuovo elemento**, quindi selezionare il **Owin Startup class**.</span><span class="sxs-lookup"><span data-stu-id="4b59d-135">Alternatively, you can right-click the project and select **Add**, then select **New Item**, and then select the **Owin Startup class**.</span></span>

     ![](owin-startup-class-detection/_static/image3.png)

- <span data-ttu-id="4b59d-136">Sostituire il codice generato nel *Startup.cs* file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4b59d-136">Replace the generated code in the *Startup.cs* file with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]

  <span data-ttu-id="4b59d-137">Il `app.Use` espressione lambda viene usata per registrare il componente specificato middleware alla pipeline OWIN.</span><span class="sxs-lookup"><span data-stu-id="4b59d-137">The `app.Use` lambda expression is used to register the specified middleware component to the OWIN pipeline.</span></span> <span data-ttu-id="4b59d-138">In questo caso viene impostata la registrazione delle richieste in ingresso prima di rispondere alla richiesta in ingresso.</span><span class="sxs-lookup"><span data-stu-id="4b59d-138">In this case we are setting up logging of incoming requests before responding to the incoming request.</span></span> <span data-ttu-id="4b59d-139">Il `next` parametro è il delegato ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [attività](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) al componente successivo nella pipeline.</span><span class="sxs-lookup"><span data-stu-id="4b59d-139">The `next` parameter is the delegate ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) to the next component in the pipeline.</span></span> <span data-ttu-id="4b59d-140">Il `app.Run` espressione lambda associa alla pipeline di richieste in ingresso e fornisce il meccanismo di risposta.</span><span class="sxs-lookup"><span data-stu-id="4b59d-140">The `app.Run` lambda expression hooks up the pipeline to incoming requests and provides the response mechanism.</span></span>
     > [!NOTE]
     > <span data-ttu-id="4b59d-141">Nel codice precedente è stata impostata come commento il `OwinStartup` attributo e si sta affidarsi alla convenzione in esecuzione la classe denominata `Startup` .-premere ***F5*** per eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4b59d-141">In the code above we have commented out the `OwinStartup` attribute and we're relying on the convention of running the class named `Startup` .- Press ***F5*** to run the application.</span></span> <span data-ttu-id="4b59d-142">Fare clic su Aggiorna più volte.</span><span class="sxs-lookup"><span data-stu-id="4b59d-142">Hit refresh a few times.</span></span>

    <span data-ttu-id="4b59d-143">![](owin-startup-class-detection/_static/image4.png) Nota: Il numero illustrato nelle immagini in questa esercitazione non corrisponderanno il numero visualizzato.</span><span class="sxs-lookup"><span data-stu-id="4b59d-143">![](owin-startup-class-detection/_static/image4.png) Note: The number shown in the images in this tutorial will not match the number you see.</span></span> <span data-ttu-id="4b59d-144">La stringa di millisecondo viene utilizzata per visualizzare una nuova risposta quando si aggiorna la pagina.</span><span class="sxs-lookup"><span data-stu-id="4b59d-144">The millisecond string is used to show a new response when you refresh the page.</span></span>
  <span data-ttu-id="4b59d-145">È possibile visualizzare le informazioni della traccia le **Output** finestra.</span><span class="sxs-lookup"><span data-stu-id="4b59d-145">You can see the trace information in the **Output** window.</span></span>

    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a><span data-ttu-id="4b59d-146">L'aggiunta di altre classi di avvio</span><span class="sxs-lookup"><span data-stu-id="4b59d-146">Add More Startup Classes</span></span>

<span data-ttu-id="4b59d-147">In questa sezione si aggiungerà un'altra classe di avvio.</span><span class="sxs-lookup"><span data-stu-id="4b59d-147">In this section we'll add another Startup class.</span></span> <span data-ttu-id="4b59d-148">È possibile aggiungere più classe di avvio OWIN all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4b59d-148">You can add multiple OWIN startup class to your application.</span></span> <span data-ttu-id="4b59d-149">Ad esempio, è possibile creare classi di avvio per lo sviluppo, test e produzione.</span><span class="sxs-lookup"><span data-stu-id="4b59d-149">For example, you might want to create startup classes for development, testing and production.</span></span>

1. <span data-ttu-id="4b59d-150">Creare una nuova classe OWIN Startup e denominarlo `ProductionStartup`.</span><span class="sxs-lookup"><span data-stu-id="4b59d-150">Create a new OWIN Startup class and name it `ProductionStartup`.</span></span>
2. <span data-ttu-id="4b59d-151">Sostituire il codice generato con il seguente:</span><span class="sxs-lookup"><span data-stu-id="4b59d-151">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. <span data-ttu-id="4b59d-152">Premere F5 di controllo per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="4b59d-152">Press Control F5 to run the app.</span></span> <span data-ttu-id="4b59d-153">Il `OwinStartup` attributo specifica la classe di avvio di produzione viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="4b59d-153">The `OwinStartup` attribute specifies the production startup class is run.</span></span>

    ![](owin-startup-class-detection/_static/image6.png)
4. <span data-ttu-id="4b59d-154">Creare un'altra classe OWIN Startup e denominarla `TestStartup`.</span><span class="sxs-lookup"><span data-stu-id="4b59d-154">Create another OWIN Startup class and name it `TestStartup`.</span></span>
5. <span data-ttu-id="4b59d-155">Sostituire il codice generato con il seguente:</span><span class="sxs-lookup"><span data-stu-id="4b59d-155">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   <span data-ttu-id="4b59d-156">Il `OwinStartup` overload attributo precedente specifica `TestingConfiguration` come la *descrittivo* nome della classe Startup.</span><span class="sxs-lookup"><span data-stu-id="4b59d-156">The `OwinStartup` attribute overload above specifies `TestingConfiguration` as the *friendly* name of the Startup class.</span></span>
6. <span data-ttu-id="4b59d-157">Aprire il *Web. config* file e aggiungere la chiave di avvio OWIN App che specifica il nome descrittivo della classe di avvio:</span><span class="sxs-lookup"><span data-stu-id="4b59d-157">Open the *web.config* file and add the OWIN App startup key which specifies the friendly name of the Startup class:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. <span data-ttu-id="4b59d-158">Premere F5 di controllo per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="4b59d-158">Press Control F5 to run the app.</span></span> <span data-ttu-id="4b59d-159">L'elemento delle impostazioni di app ha la precedenza e il test di configurazione viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="4b59d-159">The app settings element takes precedent, and the test configuration is run.</span></span>

    ![](owin-startup-class-detection/_static/image7.png)
8. <span data-ttu-id="4b59d-160">Rimuovere il *descrittivo* nome dal `OwinStartup` attributo di `TestStartup` classe.</span><span class="sxs-lookup"><span data-stu-id="4b59d-160">Remove the *friendly* name from the `OwinStartup` attribute in the `TestStartup` class.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. <span data-ttu-id="4b59d-161">Sostituire la chiave di avvio OWIN App le *Web. config* file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4b59d-161">Replace the OWIN App startup key in the *web.config* file with the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. <span data-ttu-id="4b59d-162">Ripristinare il `OwinStartup` attributo in ogni classe per il codice di attributo predefinito generato da Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="4b59d-162">Revert the `OwinStartup` attribute in each class to the default attribute code generated by Visual Studio:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    <span data-ttu-id="4b59d-163">Ogni chiave di avvio OWIN App seguente causerà l'esecuzione della classe di produzione.</span><span class="sxs-lookup"><span data-stu-id="4b59d-163">Each of the OWIN App startup keys below will cause the production class to run.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    <span data-ttu-id="4b59d-164">L'ultima chiave di avvio specifica il metodo di configurazione di avvio.</span><span class="sxs-lookup"><span data-stu-id="4b59d-164">The last startup key specifies the startup configuration method.</span></span> <span data-ttu-id="4b59d-165">La chiave di avvio OWIN App seguente consente di modificare il nome della classe configuration per `MyConfiguration` .</span><span class="sxs-lookup"><span data-stu-id="4b59d-165">The following OWIN App startup key allows you to change the name of the configuration class to `MyConfiguration` .</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a><span data-ttu-id="4b59d-166">Usando Owinhost.exe</span><span class="sxs-lookup"><span data-stu-id="4b59d-166">Using Owinhost.exe</span></span>

1. <span data-ttu-id="4b59d-167">Sostituire il file Web. config con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="4b59d-167">Replace the Web.config file with the following markup:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   <span data-ttu-id="4b59d-168">La chiave dell'ultimo vince, pertanto in questo caso `TestStartup` è specificato.</span><span class="sxs-lookup"><span data-stu-id="4b59d-168">The last key wins, so in this case `TestStartup` is specified.</span></span>
2. <span data-ttu-id="4b59d-169">Installare Owinhost dalla console di gestione pacchetti:</span><span class="sxs-lookup"><span data-stu-id="4b59d-169">Install Owinhost from the PMC:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. <span data-ttu-id="4b59d-170">Passare alla cartella dell'applicazione (la cartella contenente il *Web. config* file) e in un prompt dei comandi e digitare:</span><span class="sxs-lookup"><span data-stu-id="4b59d-170">Navigate to the application folder (the folder containing the *Web.config* file) and in a command prompt and type:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   <span data-ttu-id="4b59d-171">Nella finestra di comando verrà visualizzato:</span><span class="sxs-lookup"><span data-stu-id="4b59d-171">The command window will show:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. <span data-ttu-id="4b59d-172">Avviare un browser con URL `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="4b59d-172">Launch a browser with the URL `http://localhost:5000/`.</span></span>

    ![](owin-startup-class-detection/_static/image8.png)

   <span data-ttu-id="4b59d-173">OwinHost rispettate le convenzioni di avvio elencate in precedenza.</span><span class="sxs-lookup"><span data-stu-id="4b59d-173">OwinHost honored the startup conventions listed above.</span></span>
5. <span data-ttu-id="4b59d-174">Nella finestra di comando, premere INVIO per uscire OwinHost.</span><span class="sxs-lookup"><span data-stu-id="4b59d-174">In the command window, press Enter to exit OwinHost.</span></span>
6. <span data-ttu-id="4b59d-175">Nel `ProductionStartup` classe, aggiungere l'attributo OwinStartup seguente che specifica un nome descrittivo del *ProductionConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="4b59d-175">In the `ProductionStartup` class, add the following OwinStartup attribute which specifies a friendly name of *ProductionConfiguration*.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. <span data-ttu-id="4b59d-176">Nel prompt dei comandi e digitare:</span><span class="sxs-lookup"><span data-stu-id="4b59d-176">In the command prompt and type:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   <span data-ttu-id="4b59d-177">La classe di avvio di produzione viene caricata.</span><span class="sxs-lookup"><span data-stu-id="4b59d-177">The Production startup class is loaded.</span></span>
    ![](owin-startup-class-detection/_static/image9.png)

   <span data-ttu-id="4b59d-178">L'applicazione dispone di più classi di avvio e in questo esempio sono stati posticipato quale classe di avvio per caricare fino al runtime.</span><span class="sxs-lookup"><span data-stu-id="4b59d-178">Our application has multiple startup classes, and in this example we have deferred which startup class to load until runtime.</span></span>
8. <span data-ttu-id="4b59d-179">Testare le opzioni di avvio di runtime seguenti:</span><span class="sxs-lookup"><span data-stu-id="4b59d-179">Test the following runtime startup options:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
