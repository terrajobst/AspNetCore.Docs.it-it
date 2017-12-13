---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: Analisi in ASP.NET Web API 2 | Documenti Microsoft
author: MikeWasson
description: Viene illustrato come abilitare la traccia di ASP.NET Web API.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f35c8a10018ce796e2d905d6ee839ff09bb380a1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="tracing-in-aspnet-web-api-2"></a><span data-ttu-id="76197-103">Analisi in ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="76197-103">Tracing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="76197-104">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="76197-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="76197-105">Quando si sta tentando di eseguire il debug di un'applicazione basata su web, non si verifica alcuna sostituzione per un set ottimo di log di traccia.</span><span class="sxs-lookup"><span data-stu-id="76197-105">When you are trying to debug a web-based application, there is no substitute for a good set of trace logs.</span></span> <span data-ttu-id="76197-106">In questa esercitazione viene illustrato come abilitare la traccia di ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="76197-106">This tutorial shows how to enable tracing in ASP.NET Web API.</span></span> <span data-ttu-id="76197-107">È possibile utilizzare questa funzionalità per il framework Web API cosa prima e dopo che viene richiamato il controller di traccia.</span><span class="sxs-lookup"><span data-stu-id="76197-107">You can use this feature to trace what the Web API framework does before and after it invokes your controller.</span></span> <span data-ttu-id="76197-108">È possibile inoltre utilizzare per tracciare il proprio codice.</span><span class="sxs-lookup"><span data-stu-id="76197-108">You can also use it to trace your own code.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="76197-109">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="76197-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="76197-110">[Visual Studio 2017](https://www.visualstudio.com/downloads/) (funziona anche con Visual Studio 2015)</span><span class="sxs-lookup"><span data-stu-id="76197-110">[Visual Studio 2017](https://www.visualstudio.com/downloads/) (also works with Visual Studio 2015)</span></span>
> - <span data-ttu-id="76197-111">Web API 2</span><span class="sxs-lookup"><span data-stu-id="76197-111">Web API 2</span></span>
> - [<span data-ttu-id="76197-112">Italiano</span><span class="sxs-lookup"><span data-stu-id="76197-112">Microsoft.AspNet.WebApi.Tracing</span></span>](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)


## <a name="enable-systemdiagnostics-tracing-in-web-api"></a><span data-ttu-id="76197-113">Abilitare la traccia in Web API System. Diagnostics</span><span class="sxs-lookup"><span data-stu-id="76197-113">Enable System.Diagnostics Tracing in Web API</span></span>

<span data-ttu-id="76197-114">In primo luogo, si creerà un nuovo progetto applicazione Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="76197-114">First, we'll create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="76197-115">In Visual Studio, dal **File** dal menu **New**, quindi **progetto**.</span><span class="sxs-lookup"><span data-stu-id="76197-115">In Visual Studio, from the **File** menu, select **New**, then **Project**.</span></span> <span data-ttu-id="76197-116">In **modelli**, **Web**selezionare **applicazione Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="76197-116">Under **Templates**, **Web**, select **ASP.NET Web Application**.</span></span>

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="76197-117">Scegliere il modello di progetto API Web.</span><span class="sxs-lookup"><span data-stu-id="76197-117">Choose the Web API project template.</span></span>

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

<span data-ttu-id="76197-118">Dal **strumenti** dal menu **Gestione pacchetti libreria**, quindi **pacchetto Console di gestione**.</span><span class="sxs-lookup"><span data-stu-id="76197-118">From the **Tools** menu, select **Library Package Manager**, then **Package Manage Console**.</span></span>

<span data-ttu-id="76197-119">Nella finestra della Console di gestione pacchetti, digitare i comandi seguenti.</span><span class="sxs-lookup"><span data-stu-id="76197-119">In the Package Manager Console window, type the following commands.</span></span>

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

<span data-ttu-id="76197-120">Il primo comando installa il pacchetto di analisi Web API più recente.</span><span class="sxs-lookup"><span data-stu-id="76197-120">The first command installs the latest Web API tracing package.</span></span> <span data-ttu-id="76197-121">Aggiorna anche i pacchetti di Web API core.</span><span class="sxs-lookup"><span data-stu-id="76197-121">It also updates the core Web API packages.</span></span> <span data-ttu-id="76197-122">Il secondo comando Aggiorna il pacchetto WebApi.WebHost alla versione più recente.</span><span class="sxs-lookup"><span data-stu-id="76197-122">The second command updates the WebApi.WebHost package to the latest version.</span></span>

> [!NOTE]
> <span data-ttu-id="76197-123">Se si desidera utilizzare una versione specifica dell'API Web, utilizzare il parametro - flag di versione quando si installa il pacchetto di analisi.</span><span class="sxs-lookup"><span data-stu-id="76197-123">If you want to target a specific version of Web API, use the -Version flag when you install the tracing package.</span></span>


<span data-ttu-id="76197-124">Aprire il file WebApiConfig.cs nell'App\_cartella di avvio.</span><span class="sxs-lookup"><span data-stu-id="76197-124">Open the file WebApiConfig.cs in the App\_Start folder.</span></span> <span data-ttu-id="76197-125">Aggiungere il codice seguente per il **registrare** metodo.</span><span class="sxs-lookup"><span data-stu-id="76197-125">Add the following code to the **Register** method.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

<span data-ttu-id="76197-126">Questo codice aggiunge il [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/en-us/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) classe per la pipeline di Web API.</span><span class="sxs-lookup"><span data-stu-id="76197-126">This code adds the [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/en-us/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) class to the Web API pipeline.</span></span> <span data-ttu-id="76197-127">Il **SystemDiagnosticsTraceWriter** classe scrive tracce a [Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace).</span><span class="sxs-lookup"><span data-stu-id="76197-127">The **SystemDiagnosticsTraceWriter** class writes traces to [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace).</span></span>

<span data-ttu-id="76197-128">Per visualizzare le tracce, eseguire l'applicazione nel debugger.</span><span class="sxs-lookup"><span data-stu-id="76197-128">To see the traces, run the application in the debugger.</span></span> <span data-ttu-id="76197-129">Nel browser, passare a `/api/values`.</span><span class="sxs-lookup"><span data-stu-id="76197-129">In the browser, navigate to `/api/values`.</span></span>

![](tracing-in-aspnet-web-api/_static/image5.png)

<span data-ttu-id="76197-130">Le istruzioni di traccia vengono scritti nella finestra di Output in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="76197-130">The trace statements are written to the Output window in Visual Studio.</span></span> <span data-ttu-id="76197-131">(Dal **vista** dal menu **Output**).</span><span class="sxs-lookup"><span data-stu-id="76197-131">(From the **View** menu, select **Output**).</span></span>

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

<span data-ttu-id="76197-132">Poiché **SystemDiagnosticsTraceWriter** scrive tracce a **Trace**, è possibile registrare i listener di traccia aggiuntivi, ad esempio, per scrivere le tracce in un file di log.</span><span class="sxs-lookup"><span data-stu-id="76197-132">Because **SystemDiagnosticsTraceWriter** writes traces to **System.Diagnostics.Trace**, you can register additional trace listeners; for example, to write traces to a log file.</span></span> <span data-ttu-id="76197-133">Per ulteriori informazioni sui writer di traccia, vedere il [listener di traccia](https://msdn.microsoft.com/en-us/library/4y5y10s7.aspx) su MSDN.</span><span class="sxs-lookup"><span data-stu-id="76197-133">For more information about trace writers, see the [Trace Listeners](https://msdn.microsoft.com/en-us/library/4y5y10s7.aspx) topic on MSDN.</span></span>

### <a name="configuring-systemdiagnosticstracewriter"></a><span data-ttu-id="76197-134">Configurazione SystemDiagnosticsTraceWriter</span><span class="sxs-lookup"><span data-stu-id="76197-134">Configuring SystemDiagnosticsTraceWriter</span></span>

<span data-ttu-id="76197-135">Il codice seguente viene illustrato come configurare il writer di traccia.</span><span class="sxs-lookup"><span data-stu-id="76197-135">The following code shows how to configure the trace writer.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="76197-136">Sono disponibili due impostazioni che è possibile controllare:</span><span class="sxs-lookup"><span data-stu-id="76197-136">There are two settings that you can control:</span></span>

- <span data-ttu-id="76197-137">IsVerbose: Se è false, ogni traccia contiene informazioni minime.</span><span class="sxs-lookup"><span data-stu-id="76197-137">IsVerbose: If false, each trace contains minimal information.</span></span> <span data-ttu-id="76197-138">Se true, le tracce includere altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="76197-138">If true, traces include more information.</span></span>
- <span data-ttu-id="76197-139">MinimumLevel: Imposta il livello minimo di traccia.</span><span class="sxs-lookup"><span data-stu-id="76197-139">MinimumLevel: Sets the minimum trace level.</span></span> <span data-ttu-id="76197-140">Livelli di traccia, in ordine, sono di Debug, Info, Warn, errore ed errore irreversibile.</span><span class="sxs-lookup"><span data-stu-id="76197-140">Trace levels, in order, are Debug, Info, Warn, Error, and Fatal.</span></span>

## <a name="adding-traces-to-your-web-api-application"></a><span data-ttu-id="76197-141">Aggiunta di tracce per l'applicazione Web API</span><span class="sxs-lookup"><span data-stu-id="76197-141">Adding Traces to Your Web API Application</span></span>

<span data-ttu-id="76197-142">Aggiunta di un writer di traccia consente di accedere immediatamente per le tracce create tramite la pipeline di Web API.</span><span class="sxs-lookup"><span data-stu-id="76197-142">Adding a trace writer gives you immediate access to the traces created by the Web API pipeline.</span></span> <span data-ttu-id="76197-143">Inoltre, è possibile utilizzare il writer di traccia per il proprio codice di traccia:</span><span class="sxs-lookup"><span data-stu-id="76197-143">You can also use the trace writer to trace your own code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="76197-144">Per ottenere il writer di traccia, chiamare **HttpConfiguration.Services.GetTraceWriter**.</span><span class="sxs-lookup"><span data-stu-id="76197-144">To get the trace writer, call **HttpConfiguration.Services.GetTraceWriter**.</span></span> <span data-ttu-id="76197-145">Da un controller, questo metodo è accessibile tramite il **ApiController.Configuration** proprietà.</span><span class="sxs-lookup"><span data-stu-id="76197-145">From a controller, this method is accessible through the **ApiController.Configuration** property.</span></span>

<span data-ttu-id="76197-146">Per creare una traccia, è possibile chiamare il **ITraceWriter.Trace** metodo direttamente, ma la [ITraceWriterExtensions](https://msdn.microsoft.com/en-us/library/system.web.http.tracing.itracewriterextensions.aspx) classe definisce alcuni metodi di estensione che sono più descrittivi.</span><span class="sxs-lookup"><span data-stu-id="76197-146">To write a trace, you can call the **ITraceWriter.Trace** method directly, but the [ITraceWriterExtensions](https://msdn.microsoft.com/en-us/library/system.web.http.tracing.itracewriterextensions.aspx) class defines some extension methods that are more friendly.</span></span> <span data-ttu-id="76197-147">Ad esempio, il **Info** metodo illustrato in precedenza viene creata una traccia con livello di traccia **Info**.</span><span class="sxs-lookup"><span data-stu-id="76197-147">For example, the **Info** method shown above creates a trace with trace level **Info**.</span></span>

## <a name="web-api-tracing-infrastructure"></a><span data-ttu-id="76197-148">Infrastruttura di analisi Web API</span><span class="sxs-lookup"><span data-stu-id="76197-148">Web API Tracing Infrastructure</span></span>

<span data-ttu-id="76197-149">In questa sezione viene descritto come scrivere un writer di traccia personalizzato per l'API Web.</span><span class="sxs-lookup"><span data-stu-id="76197-149">This section describes how to write a custom trace writer for Web API.</span></span>

<span data-ttu-id="76197-150">Il pacchetto italiano è incorporato nell'infrastruttura di traccia più generale in Web API.</span><span class="sxs-lookup"><span data-stu-id="76197-150">The Microsoft.AspNet.WebApi.Tracing package is built on top of a more general tracing infrastructure in Web API.</span></span> <span data-ttu-id="76197-151">Anziché utilizzare italiano, è inoltre possibile collegare in qualche altra libreria di analisi/effettuato l'accesso, ad esempio [NLog](http://nlog-project.org/) o [log4net](http://logging.apache.org/log4net/).</span><span class="sxs-lookup"><span data-stu-id="76197-151">Instead of using Microsoft.AspNet.WebApi.Tracing, you can also plug in some other tracing/loggin library, such as [NLog](http://nlog-project.org/) or [log4net](http://logging.apache.org/log4net/).</span></span>

<span data-ttu-id="76197-152">Per raccogliere le tracce, implementare il **ITraceWriter** interfaccia.</span><span class="sxs-lookup"><span data-stu-id="76197-152">To collect traces, implement the **ITraceWriter** interface.</span></span> <span data-ttu-id="76197-153">Di seguito è riportato un esempio:</span><span class="sxs-lookup"><span data-stu-id="76197-153">Here is a simple example:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="76197-154">Il **ITraceWriter.Trace** metodo crea una traccia.</span><span class="sxs-lookup"><span data-stu-id="76197-154">The **ITraceWriter.Trace** method creates a trace.</span></span> <span data-ttu-id="76197-155">Il chiamante specifica un livello di traccia e di categoria.</span><span class="sxs-lookup"><span data-stu-id="76197-155">The caller specifies a category and trace level.</span></span> <span data-ttu-id="76197-156">La categoria può essere qualsiasi stringa definita dall'utente.</span><span class="sxs-lookup"><span data-stu-id="76197-156">The category can be any user-defined string.</span></span> <span data-ttu-id="76197-157">L'implementazione di **traccia** deve eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="76197-157">Your implementation of **Trace** should do the following:</span></span>

1. <span data-ttu-id="76197-158">Creare un nuovo **TraceRecord**.</span><span class="sxs-lookup"><span data-stu-id="76197-158">Create a new **TraceRecord**.</span></span> <span data-ttu-id="76197-159">Inizializzare con la richiesta, categoria e livello di traccia, come illustrato.</span><span class="sxs-lookup"><span data-stu-id="76197-159">Initialize it with the request, category, and trace level, as shown.</span></span> <span data-ttu-id="76197-160">Questi valori vengono forniti dal chiamante.</span><span class="sxs-lookup"><span data-stu-id="76197-160">These values are provided by the caller.</span></span>
2. <span data-ttu-id="76197-161">Richiamare il *traceAction* delegato.</span><span class="sxs-lookup"><span data-stu-id="76197-161">Invoke the *traceAction* delegate.</span></span> <span data-ttu-id="76197-162">All'interno di questo tipo di delegato, il chiamante deve compilare il resto del **TraceRecord**.</span><span class="sxs-lookup"><span data-stu-id="76197-162">Inside this delegate, the caller is expected to fill in the rest of the **TraceRecord**.</span></span>
3. <span data-ttu-id="76197-163">Scrivere il **TraceRecord**, utilizzando qualsiasi tecnica di registrazione desiderato.</span><span class="sxs-lookup"><span data-stu-id="76197-163">Write the **TraceRecord**, using any logging technique that you like.</span></span> <span data-ttu-id="76197-164">Nell'esempio illustrato di seguito chiama semplicemente **Trace**.</span><span class="sxs-lookup"><span data-stu-id="76197-164">The example shown here simply calls into **System.Diagnostics.Trace**.</span></span>

## <a name="setting-the-trace-writer"></a><span data-ttu-id="76197-165">Impostare il Writer di traccia</span><span class="sxs-lookup"><span data-stu-id="76197-165">Setting the Trace Writer</span></span>

<span data-ttu-id="76197-166">Per abilitare la traccia, è necessario configurare l'API Web per utilizzare il **ITraceWriter** implementazione.</span><span class="sxs-lookup"><span data-stu-id="76197-166">To enable tracing, you must configure Web API to use your **ITraceWriter** implementation.</span></span> <span data-ttu-id="76197-167">È possibile tramite il **HttpConfiguration** dell'oggetto, come illustrato nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="76197-167">You do this through the **HttpConfiguration** object, as shown in the following code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="76197-168">Writer di traccia solo uno può essere attivo.</span><span class="sxs-lookup"><span data-stu-id="76197-168">Only one trace writer can be active.</span></span> <span data-ttu-id="76197-169">Per impostazione predefinita, set di API Web un &quot;NOOP&quot; traccia che non esegue alcuna operazione.</span><span class="sxs-lookup"><span data-stu-id="76197-169">By default, Web API sets a &quot;no-op&quot; tracer that does nothing.</span></span> <span data-ttu-id="76197-170">(Il &quot;NOOP&quot; traccia esiste in modo che l'analisi del codice non è necessario controllare se il writer di traccia è **null** prima della scrittura di una traccia.)</span><span class="sxs-lookup"><span data-stu-id="76197-170">(The &quot;no-op&quot; tracer exists so that tracing code does not have to check whether the trace writer is **null** before writing a trace.)</span></span>

## <a name="how-web-api-tracing-works"></a><span data-ttu-id="76197-171">Ricerca Web API Tracing Works</span><span class="sxs-lookup"><span data-stu-id="76197-171">How Web API Tracing Works</span></span>

<span data-ttu-id="76197-172">Traccia negli utilizzi di API Web viene utilizzata un'API Web in un *facciata* motivo: quando è attivata, API Web esegue il wrapping di varie parti della pipeline di richieste con le classi che eseguono chiamate di traccia.</span><span class="sxs-lookup"><span data-stu-id="76197-172">Tracing in Web API uses a in Web API uses a *facade* pattern: When tracing is enabled, Web API wraps various parts of the request pipeline with classes that perform trace calls.</span></span>

<span data-ttu-id="76197-173">Ad esempio, quando si seleziona un controller, la pipeline utilizza il **IHttpControllerSelector** interfaccia.</span><span class="sxs-lookup"><span data-stu-id="76197-173">For example, when selecting a controller, the pipeline uses the **IHttpControllerSelector** interface.</span></span> <span data-ttu-id="76197-174">Con la traccia è attivata, il pipleline inserisce una classe che implementa **IHttpControllerSelector** ma chiamate all'implementazione reale:</span><span class="sxs-lookup"><span data-stu-id="76197-174">With tracing enabled, the pipleline inserts a class that implements **IHttpControllerSelector** but calls through to the real implementation:</span></span>

![Traccia API Web Usa il modello di facciata.](tracing-in-aspnet-web-api/_static/image8.png)

<span data-ttu-id="76197-176">I vantaggi di questa progettazione includono:</span><span class="sxs-lookup"><span data-stu-id="76197-176">The benefits of this design include:</span></span>

- <span data-ttu-id="76197-177">Se non si aggiunge un writer di traccia, i componenti di traccia non vengono create istanze e non abbiano alcun impatto sulle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="76197-177">If you do not add a trace writer, the tracing components are not instantiated and have no performance impact.</span></span>
- <span data-ttu-id="76197-178">Se si sostituiscono servizi predefiniti, ad esempio **IHttpControllerSelector** con un'implementazione personalizzata, la traccia non è interessata, perché la traccia viene eseguita dall'oggetto wrapper.</span><span class="sxs-lookup"><span data-stu-id="76197-178">If you replace default services such as **IHttpControllerSelector** with your own custom implementation, tracing is not affected, because tracing is done by the wrapper object.</span></span>

<span data-ttu-id="76197-179">È inoltre possibile sostituire l'intero framework traccia API Web con propri framework personalizzato, sostituendo il valore predefinito **ITraceManager** servizio:</span><span class="sxs-lookup"><span data-stu-id="76197-179">You can also replace the entire Web API trace framework with your own custom framework, by replacing the default **ITraceManager** service:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="76197-180">Implementare **ITraceManager.Initialize** per inizializzare il sistema di traccia.</span><span class="sxs-lookup"><span data-stu-id="76197-180">Implement **ITraceManager.Initialize** to initialize your tracing system.</span></span> <span data-ttu-id="76197-181">Tenere presente che questa impostazione sostituisce il *intero* framework di traccia, inclusi tutto il codice di traccia che viene compilato nell'API Web.</span><span class="sxs-lookup"><span data-stu-id="76197-181">Be aware that this replaces the *entire* trace framework, including all of the tracing code that is built into Web API.</span></span>
