---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Self-hosting ASP.NET Web API 1 (c#) | Documenti Microsoft
author: MikeWasson
description: ASP.NET Web API non richiede IIS. Automatica, è possibile ospitare un'API web nel proprio processo host. In questa esercitazione viene illustrato come ospitare un'API web all'interno di una console applic...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2012
ms.topic: article
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: 564f859e73a88ac9c5f27e9b8f7409ec126642f8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="self-host-aspnet-web-api-1-c"></a><span data-ttu-id="75c9e-105">Self-hosting ASP.NET Web API 1 (c#)</span><span class="sxs-lookup"><span data-stu-id="75c9e-105">Self-Host ASP.NET Web API 1 (C#)</span></span>
====================
<span data-ttu-id="75c9e-106">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="75c9e-106">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="75c9e-107">ASP.NET Web API non richiede IIS.</span><span class="sxs-lookup"><span data-stu-id="75c9e-107">ASP.NET Web API does not require IIS.</span></span> <span data-ttu-id="75c9e-108">Automatica, è possibile ospitare un'API web nel proprio processo host.</span><span class="sxs-lookup"><span data-stu-id="75c9e-108">You can self-host a web API in your own host process.</span></span> <span data-ttu-id="75c9e-109">In questa esercitazione viene illustrato come ospitare un'API web all'interno di un'applicazione console.</span><span class="sxs-lookup"><span data-stu-id="75c9e-109">This tutorial shows how to host a web API inside a console application.</span></span>
> 
> <span data-ttu-id="75c9e-110">**Nuove applicazioni devono utilizzare OWIN per l'hosting indipendente API Web.**</span><span class="sxs-lookup"><span data-stu-id="75c9e-110">**New applications should use OWIN to self-host Web API.**</span></span> <span data-ttu-id="75c9e-111">Vedere [utilizzare OWIN per l'hosting indipendente ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="75c9e-111">See [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="75c9e-112">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="75c9e-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="75c9e-113">API Web 1</span><span class="sxs-lookup"><span data-stu-id="75c9e-113">Web API 1</span></span>
> - <span data-ttu-id="75c9e-114">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="75c9e-114">Visual Studio 2012</span></span>


## <a name="create-the-console-application-project"></a><span data-ttu-id="75c9e-115">Creare il progetto di applicazione Console</span><span class="sxs-lookup"><span data-stu-id="75c9e-115">Create the Console Application Project</span></span>

<span data-ttu-id="75c9e-116">Avviare Visual Studio e selezionare **nuovo progetto** dal **avviare** pagina.</span><span class="sxs-lookup"><span data-stu-id="75c9e-116">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="75c9e-117">O dal **File** dal menu **New** e quindi **progetto**.</span><span class="sxs-lookup"><span data-stu-id="75c9e-117">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="75c9e-118">Nel **modelli** riquadro, selezionare **modelli installati** ed espandere il **Visual c#** nodo.</span><span class="sxs-lookup"><span data-stu-id="75c9e-118">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="75c9e-119">In **Visual c#** selezionare **Windows**.</span><span class="sxs-lookup"><span data-stu-id="75c9e-119">Under **Visual C#**, select **Windows**.</span></span> <span data-ttu-id="75c9e-120">Nell'elenco dei modelli di progetto, selezionare **applicazione Console**.</span><span class="sxs-lookup"><span data-stu-id="75c9e-120">In the list of project templates, select **Console Application**.</span></span> <span data-ttu-id="75c9e-121">Denominare il progetto &quot;SelfHost&quot; e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="75c9e-121">Name the project &quot;SelfHost&quot; and click **OK**.</span></span>

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a><span data-ttu-id="75c9e-122">Impostare il Framework di destinazione (Visual Studio 2010)</span><span class="sxs-lookup"><span data-stu-id="75c9e-122">Set the Target Framework (Visual Studio 2010)</span></span>

<span data-ttu-id="75c9e-123">Se si utilizza Visual Studio 2010, è possibile modificare il framework di destinazione in .NET Framework 4.0.</span><span class="sxs-lookup"><span data-stu-id="75c9e-123">If you are using Visual Studio 2010, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="75c9e-124">(Per impostazione predefinita, le destinazioni di modello di progetto di [del profilo di .net Framework Client](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span><span class="sxs-lookup"><span data-stu-id="75c9e-124">(By default, the project template targets the [.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span></span>

<span data-ttu-id="75c9e-125">In Esplora soluzioni, fare clic sul progetto e selezionare **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="75c9e-125">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="75c9e-126">Nel **framework di destinazione** elenco a discesa elenco, modificare il framework di destinazione in .NET Framework 4.0.</span><span class="sxs-lookup"><span data-stu-id="75c9e-126">In the **Target framework** dropdown list, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="75c9e-127">Quando viene richiesto di applicare la modifica, fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="75c9e-127">When prompted to apply the change, click **Yes**.</span></span>

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a><span data-ttu-id="75c9e-128">Installare Gestione pacchetti NuGet</span><span class="sxs-lookup"><span data-stu-id="75c9e-128">Install NuGet Package Manager</span></span>

<span data-ttu-id="75c9e-129">Gestione pacchetti NuGet è il modo più semplice per aggiungere gli assembly di API Web a un progetto non ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="75c9e-129">The NuGet Package Manager is the easiest way to add the Web API assemblies to a non-ASP.NET project.</span></span>

<span data-ttu-id="75c9e-130">Per verificare se è installato Gestione pacchetti NuGet, fare clic su di **strumenti** menu in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="75c9e-130">To check if NuGet Package Manager is installed, click the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="75c9e-131">Se viene visualizzato un menu chiamato **Gestione pacchetti libreria**, è necessario Gestione pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="75c9e-131">If you see a menu item called **Library Package Manager**, then you have NuGet Package Manager.</span></span>

<span data-ttu-id="75c9e-132">Per installare Gestione pacchetti NuGet:</span><span class="sxs-lookup"><span data-stu-id="75c9e-132">To install NuGet Package Manager:</span></span>

1. <span data-ttu-id="75c9e-133">Avviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="75c9e-133">Start Visual Studio.</span></span>
2. <span data-ttu-id="75c9e-134">Dal **strumenti** dal menu **estensioni e aggiornamenti**.</span><span class="sxs-lookup"><span data-stu-id="75c9e-134">From the **Tools** menu, select **Extensions and Updates**.</span></span>
3. <span data-ttu-id="75c9e-135">Nel **estensioni e aggiornamenti** finestra di dialogo Seleziona **Online**.</span><span class="sxs-lookup"><span data-stu-id="75c9e-135">In the **Extensions and Updates** dialog, select **Online**.</span></span>
4. <span data-ttu-id="75c9e-136">Se non viene visualizzato "Gestione pacchetti NuGet", digitare "Gestione pacchetti nuget" nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="75c9e-136">If you don't see "NuGet Package Manager", type "nuget package manager" in the search box.</span></span>
5. <span data-ttu-id="75c9e-137">Selezionare Gestione pacchetti NuGet e fare clic su **scaricare**.</span><span class="sxs-lookup"><span data-stu-id="75c9e-137">Select the NuGet Package Manager and click **Download**.</span></span>
6. <span data-ttu-id="75c9e-138">Al termine del download, verrà richiesto di installare.</span><span class="sxs-lookup"><span data-stu-id="75c9e-138">After the download completes, you will be prompted to install.</span></span>
7. <span data-ttu-id="75c9e-139">Al termine dell'installazione, potrebbe essere necessario riavviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="75c9e-139">After the installation completes, you might be prompted to restart Visual Studio.</span></span>

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a><span data-ttu-id="75c9e-140">Aggiungere il pacchetto NuGet di API Web</span><span class="sxs-lookup"><span data-stu-id="75c9e-140">Add the Web API NuGet Package</span></span>

<span data-ttu-id="75c9e-141">Dopo l'installazione di gestione pacchetti NuGet, aggiungere il pacchetto di Web API Self-Host al progetto.</span><span class="sxs-lookup"><span data-stu-id="75c9e-141">After NuGet Package Manager is installed, add the Web API Self-Host package to your project.</span></span>

1. <span data-ttu-id="75c9e-142">Dal **strumenti** dal menu **Gestione pacchetti libreria**.</span><span class="sxs-lookup"><span data-stu-id="75c9e-142">From the **Tools** menu, select **Library Package Manager**.</span></span> <span data-ttu-id="75c9e-143">*Nota*: se si menu non viene visualizzato questo elemento, assicurarsi che tale gestione pacchetti NuGet è installato correttamente.</span><span class="sxs-lookup"><span data-stu-id="75c9e-143">*Note*: If do you not see this menu item, make sure that NuGet Package Manager installed correctly.</span></span>
2. <span data-ttu-id="75c9e-144">Selezionare **Gestisci pacchetti NuGet per la soluzione...**</span><span class="sxs-lookup"><span data-stu-id="75c9e-144">Select **Manage NuGet Packages for Solution...**</span></span>
3. <span data-ttu-id="75c9e-145">Nel **Gestisci pacchetti NugGet** finestra di dialogo Seleziona **Online**.</span><span class="sxs-lookup"><span data-stu-id="75c9e-145">In the **Manage NugGet Packages** dialog, select **Online**.</span></span>
4. <span data-ttu-id="75c9e-146">Nella casella di ricerca, digitare &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span><span class="sxs-lookup"><span data-stu-id="75c9e-146">In the search box, type &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span></span>
5. <span data-ttu-id="75c9e-147">Selezionare il pacchetto di ASP.NET Web API Self Host e fare clic su **installare**.</span><span class="sxs-lookup"><span data-stu-id="75c9e-147">Select the ASP.NET Web API Self Host package and click **Install**.</span></span>
6. <span data-ttu-id="75c9e-148">Dopo aver installato il pacchetto, fare clic su **chiudere** per chiudere la finestra di dialogo.</span><span class="sxs-lookup"><span data-stu-id="75c9e-148">After the package installs, click **Close** to close the dialog.</span></span>

> [!NOTE]
> <span data-ttu-id="75c9e-149">Assicurarsi di installare il pacchetto denominato Microsoft.AspNet.WebApi.SelfHost, non AspNetWebApi.SelfHost.</span><span class="sxs-lookup"><span data-stu-id="75c9e-149">Make sure to install the package named Microsoft.AspNet.WebApi.SelfHost, not AspNetWebApi.SelfHost.</span></span>


![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="75c9e-150">Creare il modello e il Controller</span><span class="sxs-lookup"><span data-stu-id="75c9e-150">Create the Model and Controller</span></span>

<span data-ttu-id="75c9e-151">In questa esercitazione Usa le classi di modello e il controller stesso come il [Introduzione](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) esercitazione.</span><span class="sxs-lookup"><span data-stu-id="75c9e-151">This tutorial uses the same model and controller classes as the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="75c9e-152">Aggiungere una classe pubblica denominata `Product`.</span><span class="sxs-lookup"><span data-stu-id="75c9e-152">Add a public class named `Product`.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

<span data-ttu-id="75c9e-153">Aggiungere una classe pubblica denominata `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="75c9e-153">Add a public class named `ProductsController`.</span></span> <span data-ttu-id="75c9e-154">Derivare la classe da **System.Web.Http.ApiController**.</span><span class="sxs-lookup"><span data-stu-id="75c9e-154">Derive this class from **System.Web.Http.ApiController**.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

<span data-ttu-id="75c9e-155">Per ulteriori informazioni sul codice in questo controller, vedere il [Introduzione](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) esercitazione.</span><span class="sxs-lookup"><span data-stu-id="75c9e-155">For more information about the code in this controller, see the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span> <span data-ttu-id="75c9e-156">Il controller definisce tre operazioni GET:</span><span class="sxs-lookup"><span data-stu-id="75c9e-156">This controller defines three GET actions:</span></span>

| <span data-ttu-id="75c9e-157">URI</span><span class="sxs-lookup"><span data-stu-id="75c9e-157">URI</span></span> | <span data-ttu-id="75c9e-158">Descrizione</span><span class="sxs-lookup"><span data-stu-id="75c9e-158">Description</span></span> |
| --- | --- |
| <span data-ttu-id="75c9e-159">prodotti/api /</span><span class="sxs-lookup"><span data-stu-id="75c9e-159">/api/products</span></span> | <span data-ttu-id="75c9e-160">Ottenere un elenco di tutti i prodotti.</span><span class="sxs-lookup"><span data-stu-id="75c9e-160">Get a list of all products.</span></span> |
| <span data-ttu-id="75c9e-161">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="75c9e-161">/api/products/*id*</span></span> | <span data-ttu-id="75c9e-162">Ottenere un prodotto in base all'ID.</span><span class="sxs-lookup"><span data-stu-id="75c9e-162">Get a product by ID.</span></span> |
| <span data-ttu-id="75c9e-163">/api/products/?category=*category*</span><span class="sxs-lookup"><span data-stu-id="75c9e-163">/api/products/?category=*category*</span></span> | <span data-ttu-id="75c9e-164">Ottenere un elenco di prodotti per categoria.</span><span class="sxs-lookup"><span data-stu-id="75c9e-164">Get a list of products by category.</span></span> |

## <a name="host-the-web-api"></a><span data-ttu-id="75c9e-165">L'API Web host</span><span class="sxs-lookup"><span data-stu-id="75c9e-165">Host the Web API</span></span>

<span data-ttu-id="75c9e-166">Aprire il file Program.cs e aggiungere le seguenti istruzioni using:</span><span class="sxs-lookup"><span data-stu-id="75c9e-166">Open the file Program.cs and add the following using statements:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

<span data-ttu-id="75c9e-167">Aggiungere il codice seguente per il **programma** classe.</span><span class="sxs-lookup"><span data-stu-id="75c9e-167">Add the following code to the **Program** class.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a><span data-ttu-id="75c9e-168">(Facoltativo) Aggiungere una prenotazione di Namespace URL HTTP</span><span class="sxs-lookup"><span data-stu-id="75c9e-168">(Optional) Add an HTTP URL Namespace Reservation</span></span>

<span data-ttu-id="75c9e-169">Questa applicazione è in ascolto `http://localhost:8080/`.</span><span class="sxs-lookup"><span data-stu-id="75c9e-169">This application listens to `http://localhost:8080/`.</span></span> <span data-ttu-id="75c9e-170">Per impostazione predefinita, l'ascolto su un particolare indirizzo HTTP richiede privilegi di amministratore.</span><span class="sxs-lookup"><span data-stu-id="75c9e-170">By default, listening at a particular HTTP address requires administrator privileges.</span></span> <span data-ttu-id="75c9e-171">Quando si esegue l'esercitazione, pertanto, potrebbe verificarsi questo errore: "HTTP può non registrare l'URL http://+:8080/" sono disponibili due modi per evitare questo errore:</span><span class="sxs-lookup"><span data-stu-id="75c9e-171">When you run the tutorial, therefore, you may get this error: "HTTP could not register URL http://+:8080/" There are two ways to avoid this error:</span></span>

- <span data-ttu-id="75c9e-172">Eseguire Visual Studio con autorizzazioni di amministratore con privilegi elevati, o</span><span class="sxs-lookup"><span data-stu-id="75c9e-172">Run Visual Studio with elevated administrator permissions, or</span></span>
- <span data-ttu-id="75c9e-173">Utilizzare Netsh.exe per concedere le autorizzazioni dell'account per riservare l'URL.</span><span class="sxs-lookup"><span data-stu-id="75c9e-173">Use Netsh.exe to give your account permissions to reserve the URL.</span></span>

<span data-ttu-id="75c9e-174">Per l'utilizzo di Netsh.exe, aprire un prompt dei comandi con privilegi di amministratore e immettere il seguente comando: seguente comando:</span><span class="sxs-lookup"><span data-stu-id="75c9e-174">To use Netsh.exe, open a command prompt with administrator privileges and enter the following command:following command:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

<span data-ttu-id="75c9e-175">dove *computer\nome utente* è l'account utente.</span><span class="sxs-lookup"><span data-stu-id="75c9e-175">where *machine\username* is your user account.</span></span>

<span data-ttu-id="75c9e-176">Quando si è finito self-hosting, assicurarsi di eliminare la prenotazione:</span><span class="sxs-lookup"><span data-stu-id="75c9e-176">When you are finished self-hosting, be sure to delete the reservation:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a><span data-ttu-id="75c9e-177">Chiamare l'API Web da un'applicazione Client (c#)</span><span class="sxs-lookup"><span data-stu-id="75c9e-177">Call the Web API from a Client Application (C#)</span></span>

<span data-ttu-id="75c9e-178">Consente di scrivere una semplice applicazione console che chiama l'API web.</span><span class="sxs-lookup"><span data-stu-id="75c9e-178">Let's write a simple console application that calls the web API.</span></span>

<span data-ttu-id="75c9e-179">Aggiungere un nuovo progetto applicazione console alla soluzione:</span><span class="sxs-lookup"><span data-stu-id="75c9e-179">Add a new console application project to the solution:</span></span>

- <span data-ttu-id="75c9e-180">In Esplora soluzioni, la soluzione e scegliere **Aggiungi nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="75c9e-180">In Solution Explorer, right-click the solution and select **Add New Project**.</span></span>
- <span data-ttu-id="75c9e-181">Creare una nuova applicazione console denominata &quot;ClientApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="75c9e-181">Create a new console application named &quot;ClientApp&quot;.</span></span>

![](self-host-a-web-api/_static/image5.png)

<span data-ttu-id="75c9e-182">Utilizzo di gestione pacchetti di NuGet per aggiungere il pacchetto di ASP.NET Web API Core Libraries:</span><span class="sxs-lookup"><span data-stu-id="75c9e-182">Use NuGet Package Manager to add the ASP.NET Web API Core Libraries package:</span></span>

- <span data-ttu-id="75c9e-183">Dal menu Strumenti, selezionare **Gestione pacchetti libreria**.</span><span class="sxs-lookup"><span data-stu-id="75c9e-183">From the Tools menu, select **Library Package Manager**.</span></span>
- <span data-ttu-id="75c9e-184">Selezionare **Gestisci pacchetti NuGet per la soluzione...**</span><span class="sxs-lookup"><span data-stu-id="75c9e-184">Select **Manage NuGet Packages for Solution...**</span></span>
- <span data-ttu-id="75c9e-185">Nel **Gestisci pacchetti NuGet** finestra di dialogo Seleziona **Online**.</span><span class="sxs-lookup"><span data-stu-id="75c9e-185">In the **Manage NuGet Packages** dialog, select **Online**.</span></span>
- <span data-ttu-id="75c9e-186">Nella casella di ricerca, digitare &quot;webapi&quot;.</span><span class="sxs-lookup"><span data-stu-id="75c9e-186">In the search box, type &quot;Microsoft.AspNet.WebApi.Client&quot;.</span></span>
- <span data-ttu-id="75c9e-187">Selezionare il pacchetto di librerie Client di Microsoft ASP.NET Web API e fare clic su **installare**.</span><span class="sxs-lookup"><span data-stu-id="75c9e-187">Select the Microsoft ASP.NET Web API Client Libraries package and click **Install**.</span></span>

<span data-ttu-id="75c9e-188">Aggiungere un riferimento in ClientApp al progetto SelfHost:</span><span class="sxs-lookup"><span data-stu-id="75c9e-188">Add a reference in ClientApp to the SelfHost project:</span></span>

- <span data-ttu-id="75c9e-189">In Esplora soluzioni, fare clic sul progetto ClientApp.</span><span class="sxs-lookup"><span data-stu-id="75c9e-189">In Solution Explorer, right-click the ClientApp project.</span></span>
- <span data-ttu-id="75c9e-190">Selezionare **aggiungere riferimento**.</span><span class="sxs-lookup"><span data-stu-id="75c9e-190">Select **Add Reference**.</span></span>
- <span data-ttu-id="75c9e-191">Nel **gestione riferimenti** finestra di dialogo, in **soluzione**selezionare **progetti**.</span><span class="sxs-lookup"><span data-stu-id="75c9e-191">In the **Reference Manager** dialog, under **Solution**, select **Projects**.</span></span>
- <span data-ttu-id="75c9e-192">Selezionare il progetto host.</span><span class="sxs-lookup"><span data-stu-id="75c9e-192">Select the SelfHost project.</span></span>
- <span data-ttu-id="75c9e-193">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="75c9e-193">Click **OK**.</span></span>

![](self-host-a-web-api/_static/image6.png)

<span data-ttu-id="75c9e-194">Aprire il file Client/Program.cs.</span><span class="sxs-lookup"><span data-stu-id="75c9e-194">Open the Client/Program.cs file.</span></span> <span data-ttu-id="75c9e-195">Aggiungere il seguente **utilizzando** istruzione:</span><span class="sxs-lookup"><span data-stu-id="75c9e-195">Add the following **using** statement:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

<span data-ttu-id="75c9e-196">Aggiungere un valore statico **HttpClient** istanza:</span><span class="sxs-lookup"><span data-stu-id="75c9e-196">Add a static **HttpClient** instance:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

<span data-ttu-id="75c9e-197">Aggiungere i metodi seguenti per elencare tutti i prodotti, un prodotto dall'ID di elenco ed elencano i prodotti per categoria.</span><span class="sxs-lookup"><span data-stu-id="75c9e-197">Add the following methods to list all products, list a product by ID, and list products by category.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

<span data-ttu-id="75c9e-198">Ognuno di questi metodi segue il modello stesso:</span><span class="sxs-lookup"><span data-stu-id="75c9e-198">Each of these methods follows the same pattern:</span></span>

1. <span data-ttu-id="75c9e-199">Chiamare **HttpClient.GetAsync** per inviare una richiesta GET all'URI appropriato.</span><span class="sxs-lookup"><span data-stu-id="75c9e-199">Call **HttpClient.GetAsync** to send a GET request to the appropriate URI.</span></span>
2. <span data-ttu-id="75c9e-200">Chiamare **HttpResponseMessage.EnsureSuccessStatusCode**.</span><span class="sxs-lookup"><span data-stu-id="75c9e-200">Call **HttpResponseMessage.EnsureSuccessStatusCode**.</span></span> <span data-ttu-id="75c9e-201">Questo metodo genera un'eccezione se lo stato della risposta HTTP è un codice di errore.</span><span class="sxs-lookup"><span data-stu-id="75c9e-201">This method throws an exception if the HTTP response status is an error code.</span></span>
3. <span data-ttu-id="75c9e-202">Chiamare **ReadAsAsync&lt;T&gt;**  per deserializzare un tipo CLR dalla risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="75c9e-202">Call **ReadAsAsync&lt;T&gt;** to deserialize a CLR type from the HTTP response.</span></span> <span data-ttu-id="75c9e-203">Questo metodo è un metodo di estensione definito in **System.Net.Http.HttpContentExtensions**.</span><span class="sxs-lookup"><span data-stu-id="75c9e-203">This method is an extension method, defined in **System.Net.Http.HttpContentExtensions**.</span></span>

<span data-ttu-id="75c9e-204">Il **GetAsync** e **ReadAsAsync** metodi sono entrambi asincrona.</span><span class="sxs-lookup"><span data-stu-id="75c9e-204">The **GetAsync** and **ReadAsAsync** methods are both asynchronous.</span></span> <span data-ttu-id="75c9e-205">Restituiscono **attività** gli oggetti che rappresentano l'operazione asincrona.</span><span class="sxs-lookup"><span data-stu-id="75c9e-205">They return **Task** objects that represent the asynchronous operation.</span></span> <span data-ttu-id="75c9e-206">Recupero di **risultato** proprietà blocca il thread fino al completamento dell'operazione.</span><span class="sxs-lookup"><span data-stu-id="75c9e-206">Getting the **Result** property blocks the thread until the operation completes.</span></span>

<span data-ttu-id="75c9e-207">Per ulteriori informazioni sull'utilizzo HttpClient, inclusa la modalità di chiamate non di blocco, vedere [la chiamata a una Web API da un Client .NET](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="75c9e-207">For more information about using HttpClient, including how to make non-blocking calls, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="75c9e-208">Prima di chiamare questi metodi, impostare la proprietà BaseAddress sull'istanza HttpClient per "`http://localhost:8080`".</span><span class="sxs-lookup"><span data-stu-id="75c9e-208">Before calling these methods, set the BaseAddress property on the HttpClient instance to "`http://localhost:8080`".</span></span> <span data-ttu-id="75c9e-209">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="75c9e-209">For example:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

<span data-ttu-id="75c9e-210">Questo dovrebbe output riportato di seguito.</span><span class="sxs-lookup"><span data-stu-id="75c9e-210">This should output the following.</span></span> <span data-ttu-id="75c9e-211">(Ricordarsi di avviare l'applicazione host).</span><span class="sxs-lookup"><span data-stu-id="75c9e-211">(Remember to run the SelfHost application first.)</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
