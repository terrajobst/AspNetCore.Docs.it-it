---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: Traccia nelle API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: Viene illustrato come abilitare la traccia nell'API Web ASP.NET.
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 426e3500c504a651a325eec021e2d9e8b9cc1d32
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795280"
---
<a name="tracing-in-aspnet-web-api-2"></a><span data-ttu-id="67bb3-103">Traccia in ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="67bb3-103">Tracing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="67bb3-104">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="67bb3-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="67bb3-105">Quando si tenta di eseguire il debug di un'applicazione basata su web, non vi è alcuna sostituzione di un set di log di traccia valido.</span><span class="sxs-lookup"><span data-stu-id="67bb3-105">When you are trying to debug a web-based application, there is no substitute for a good set of trace logs.</span></span> <span data-ttu-id="67bb3-106">Questa esercitazione illustra come abilitare la traccia nell'API Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="67bb3-106">This tutorial shows how to enable tracing in ASP.NET Web API.</span></span> <span data-ttu-id="67bb3-107">È possibile usare questa funzionalità per tracciare il framework API Web del funzionamento di prima e dopo aver richiamato il controller.</span><span class="sxs-lookup"><span data-stu-id="67bb3-107">You can use this feature to trace what the Web API framework does before and after it invokes your controller.</span></span> <span data-ttu-id="67bb3-108">È anche possibile usarlo per tracciare il proprio codice.</span><span class="sxs-lookup"><span data-stu-id="67bb3-108">You can also use it to trace your own code.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="67bb3-109">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="67bb3-109">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="67bb3-110">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) (funziona anche con Visual Studio 2015)</span><span class="sxs-lookup"><span data-stu-id="67bb3-110">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) (also works with Visual Studio 2015)</span></span>
> - <span data-ttu-id="67bb3-111">API Web 2</span><span class="sxs-lookup"><span data-stu-id="67bb3-111">Web API 2</span></span>
> - [<span data-ttu-id="67bb3-112">Microsoft.AspNet.WebApi.Tracing</span><span class="sxs-lookup"><span data-stu-id="67bb3-112">Microsoft.AspNet.WebApi.Tracing</span></span>](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)

## <a name="enable-systemdiagnostics-tracing-in-web-api"></a><span data-ttu-id="67bb3-113">Abilitare la traccia in Web API System. Diagnostics</span><span class="sxs-lookup"><span data-stu-id="67bb3-113">Enable System.Diagnostics Tracing in Web API</span></span>

<span data-ttu-id="67bb3-114">In primo luogo, si creerà un nuovo progetto di applicazione Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="67bb3-114">First, we'll create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="67bb3-115">In Visual Studio, dai **File** dal menu **New** > **progetto**.</span><span class="sxs-lookup"><span data-stu-id="67bb3-115">In Visual Studio, from the **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="67bb3-116">Sotto **modelli**, **Web**, selezionare **applicazione Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="67bb3-116">Under **Templates**, **Web**, select **ASP.NET Web Application**.</span></span>

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="67bb3-117">Scegliere il modello di progetto API Web.</span><span class="sxs-lookup"><span data-stu-id="67bb3-117">Choose the Web API project template.</span></span>

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

<span data-ttu-id="67bb3-118">Dal **degli strumenti** dal menu **Library Package Manager**, quindi **Console di gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="67bb3-118">From the **Tools** menu, select **Library Package Manager**, then **Package Manage Console**.</span></span>

<span data-ttu-id="67bb3-119">Nella finestra della Console di gestione pacchetti, digitare i comandi seguenti.</span><span class="sxs-lookup"><span data-stu-id="67bb3-119">In the Package Manager Console window, type the following commands.</span></span>

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

<span data-ttu-id="67bb3-120">Il primo comando consente di installare il pacchetto più recente di traccia API Web.</span><span class="sxs-lookup"><span data-stu-id="67bb3-120">The first command installs the latest Web API tracing package.</span></span> <span data-ttu-id="67bb3-121">Aggiorna anche i pacchetti di API Web di base.</span><span class="sxs-lookup"><span data-stu-id="67bb3-121">It also updates the core Web API packages.</span></span> <span data-ttu-id="67bb3-122">Il secondo comando Aggiorna il pacchetto WebApi.WebHost alla versione più recente.</span><span class="sxs-lookup"><span data-stu-id="67bb3-122">The second command updates the WebApi.WebHost package to the latest version.</span></span>

> [!NOTE]
> <span data-ttu-id="67bb3-123">Se si vuole destinare una versione specifica di API Web, usare l'opzione - flag di versione quando si installa il pacchetto di analisi.</span><span class="sxs-lookup"><span data-stu-id="67bb3-123">If you want to target a specific version of Web API, use the -Version flag when you install the tracing package.</span></span>


<span data-ttu-id="67bb3-124">Aprire il file WebApiConfig.cs nell'App\_cartella di avvio.</span><span class="sxs-lookup"><span data-stu-id="67bb3-124">Open the file WebApiConfig.cs in the App\_Start folder.</span></span> <span data-ttu-id="67bb3-125">Aggiungere il codice seguente per il **registrare** (metodo).</span><span class="sxs-lookup"><span data-stu-id="67bb3-125">Add the following code to the **Register** method.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

<span data-ttu-id="67bb3-126">Questo codice aggiunge il [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) classe per la pipeline di Web API.</span><span class="sxs-lookup"><span data-stu-id="67bb3-126">This code adds the [SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) class to the Web API pipeline.</span></span> <span data-ttu-id="67bb3-127">Il **SystemDiagnosticsTraceWriter** classe scrive tracce [Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).</span><span class="sxs-lookup"><span data-stu-id="67bb3-127">The **SystemDiagnosticsTraceWriter** class writes traces to [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace).</span></span>

<span data-ttu-id="67bb3-128">Per visualizzare le tracce, eseguire l'applicazione nel debugger.</span><span class="sxs-lookup"><span data-stu-id="67bb3-128">To see the traces, run the application in the debugger.</span></span> <span data-ttu-id="67bb3-129">Nel browser, passare a `/api/values`.</span><span class="sxs-lookup"><span data-stu-id="67bb3-129">In the browser, navigate to `/api/values`.</span></span>

![](tracing-in-aspnet-web-api/_static/image5.png)

<span data-ttu-id="67bb3-130">Le istruzioni di traccia vengono scritti nella finestra di Output in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="67bb3-130">The trace statements are written to the Output window in Visual Studio.</span></span> <span data-ttu-id="67bb3-131">(Dal **View** dal menu **Output**).</span><span class="sxs-lookup"><span data-stu-id="67bb3-131">(From the **View** menu, select **Output**).</span></span>

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

<span data-ttu-id="67bb3-132">In quanto **SystemDiagnosticsTraceWriter** scrive tracce **Trace**, è possibile registrare i listener di traccia aggiuntivi, ad esempio, per scrivere le tracce in un file di log.</span><span class="sxs-lookup"><span data-stu-id="67bb3-132">Because **SystemDiagnosticsTraceWriter** writes traces to **System.Diagnostics.Trace**, you can register additional trace listeners; for example, to write traces to a log file.</span></span> <span data-ttu-id="67bb3-133">Per altre informazioni sui writer di traccia, vedere la [listener di traccia](https://msdn.microsoft.com/library/4y5y10s7.aspx) argomento in MSDN.</span><span class="sxs-lookup"><span data-stu-id="67bb3-133">For more information about trace writers, see the [Trace Listeners](https://msdn.microsoft.com/library/4y5y10s7.aspx) topic on MSDN.</span></span>

### <a name="configuring-systemdiagnosticstracewriter"></a><span data-ttu-id="67bb3-134">Configurazione SystemDiagnosticsTraceWriter</span><span class="sxs-lookup"><span data-stu-id="67bb3-134">Configuring SystemDiagnosticsTraceWriter</span></span>

<span data-ttu-id="67bb3-135">Il codice seguente viene illustrato come configurare il writer di traccia.</span><span class="sxs-lookup"><span data-stu-id="67bb3-135">The following code shows how to configure the trace writer.</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="67bb3-136">Sono disponibili due impostazioni che è possibile controllare:</span><span class="sxs-lookup"><span data-stu-id="67bb3-136">There are two settings that you can control:</span></span>

- <span data-ttu-id="67bb3-137">IsVerbose: Se è false, ogni traccia contiene informazioni minime.</span><span class="sxs-lookup"><span data-stu-id="67bb3-137">IsVerbose: If false, each trace contains minimal information.</span></span> <span data-ttu-id="67bb3-138">Se true, le tracce includono altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="67bb3-138">If true, traces include more information.</span></span>
- <span data-ttu-id="67bb3-139">MinimumLevel: Imposta il livello minimo di traccia.</span><span class="sxs-lookup"><span data-stu-id="67bb3-139">MinimumLevel: Sets the minimum trace level.</span></span> <span data-ttu-id="67bb3-140">Livelli di traccia, in ordine, sono Debug, Info, Warn, errore ed errore irreversibile.</span><span class="sxs-lookup"><span data-stu-id="67bb3-140">Trace levels, in order, are Debug, Info, Warn, Error, and Fatal.</span></span>

## <a name="adding-traces-to-your-web-api-application"></a><span data-ttu-id="67bb3-141">Aggiunta di tracce per l'applicazione API Web</span><span class="sxs-lookup"><span data-stu-id="67bb3-141">Adding Traces to Your Web API Application</span></span>

<span data-ttu-id="67bb3-142">Aggiunta di un writer di traccia offre l'accesso immediato alle tracce create tramite la pipeline di API Web.</span><span class="sxs-lookup"><span data-stu-id="67bb3-142">Adding a trace writer gives you immediate access to the traces created by the Web API pipeline.</span></span> <span data-ttu-id="67bb3-143">È inoltre possibile utilizzare il writer di traccia per tracciare il proprio codice:</span><span class="sxs-lookup"><span data-stu-id="67bb3-143">You can also use the trace writer to trace your own code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="67bb3-144">Per ottenere il writer di traccia, chiamare **HttpConfiguration.Services.GetTraceWriter**.</span><span class="sxs-lookup"><span data-stu-id="67bb3-144">To get the trace writer, call **HttpConfiguration.Services.GetTraceWriter**.</span></span> <span data-ttu-id="67bb3-145">Da un controller, questo metodo è accessibile tramite il **ApiController.Configuration** proprietà.</span><span class="sxs-lookup"><span data-stu-id="67bb3-145">From a controller, this method is accessible through the **ApiController.Configuration** property.</span></span>

<span data-ttu-id="67bb3-146">Per creare una traccia, è possibile chiamare il **ITraceWriter.Trace** metodo direttamente, ma la [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) classe definisce alcuni metodi di estensione che sono più descrittivi.</span><span class="sxs-lookup"><span data-stu-id="67bb3-146">To write a trace, you can call the **ITraceWriter.Trace** method directly, but the [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) class defines some extension methods that are more friendly.</span></span> <span data-ttu-id="67bb3-147">Ad esempio, il **Info** metodo illustrato in precedenza crea una traccia con livello di traccia **Info**.</span><span class="sxs-lookup"><span data-stu-id="67bb3-147">For example, the **Info** method shown above creates a trace with trace level **Info**.</span></span>

## <a name="web-api-tracing-infrastructure"></a><span data-ttu-id="67bb3-148">Infrastruttura di traccia API Web</span><span class="sxs-lookup"><span data-stu-id="67bb3-148">Web API Tracing Infrastructure</span></span>

<span data-ttu-id="67bb3-149">Questa sezione descrive come scrivere un writer di traccia personalizzato per l'API Web.</span><span class="sxs-lookup"><span data-stu-id="67bb3-149">This section describes how to write a custom trace writer for Web API.</span></span>

<span data-ttu-id="67bb3-150">Il pacchetto Microsoft.AspNet.WebApi.Tracing si basa su un'infrastruttura di traccia più generale in API Web.</span><span class="sxs-lookup"><span data-stu-id="67bb3-150">The Microsoft.AspNet.WebApi.Tracing package is built on top of a more general tracing infrastructure in Web API.</span></span> <span data-ttu-id="67bb3-151">Invece di usare Microsoft.AspNet.WebApi.Tracing, è anche possibile collegare in qualche altra libreria di analisi/effettuato l'accesso, ad esempio [NLog](http://nlog-project.org/) oppure [log4net](http://logging.apache.org/log4net/).</span><span class="sxs-lookup"><span data-stu-id="67bb3-151">Instead of using Microsoft.AspNet.WebApi.Tracing, you can also plug in some other tracing/loggin library, such as [NLog](http://nlog-project.org/) or [log4net](http://logging.apache.org/log4net/).</span></span>

<span data-ttu-id="67bb3-152">Per raccogliere le tracce, implementare il **ITraceWriter** interfaccia.</span><span class="sxs-lookup"><span data-stu-id="67bb3-152">To collect traces, implement the **ITraceWriter** interface.</span></span> <span data-ttu-id="67bb3-153">Ecco un esempio semplice:</span><span class="sxs-lookup"><span data-stu-id="67bb3-153">Here is a simple example:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="67bb3-154">Il **ITraceWriter.Trace** metodo crea un oggetto trace.</span><span class="sxs-lookup"><span data-stu-id="67bb3-154">The **ITraceWriter.Trace** method creates a trace.</span></span> <span data-ttu-id="67bb3-155">Il chiamante specifica un livello di traccia e di categoria.</span><span class="sxs-lookup"><span data-stu-id="67bb3-155">The caller specifies a category and trace level.</span></span> <span data-ttu-id="67bb3-156">La categoria può essere qualsiasi stringa definita dall'utente.</span><span class="sxs-lookup"><span data-stu-id="67bb3-156">The category can be any user-defined string.</span></span> <span data-ttu-id="67bb3-157">L'implementazione di **traccia** deve eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="67bb3-157">Your implementation of **Trace** should do the following:</span></span>

1. <span data-ttu-id="67bb3-158">Creare una nuova **TraceRecord**.</span><span class="sxs-lookup"><span data-stu-id="67bb3-158">Create a new **TraceRecord**.</span></span> <span data-ttu-id="67bb3-159">Inizializzarlo con la richiesta, categoria e livello di traccia, come illustrato.</span><span class="sxs-lookup"><span data-stu-id="67bb3-159">Initialize it with the request, category, and trace level, as shown.</span></span> <span data-ttu-id="67bb3-160">Questi valori vengono forniti dal chiamante.</span><span class="sxs-lookup"><span data-stu-id="67bb3-160">These values are provided by the caller.</span></span>
2. <span data-ttu-id="67bb3-161">Richiama il *traceAction* delegare.</span><span class="sxs-lookup"><span data-stu-id="67bb3-161">Invoke the *traceAction* delegate.</span></span> <span data-ttu-id="67bb3-162">All'interno di questo delegato, il chiamante dovrà completare nel resto del **TraceRecord**.</span><span class="sxs-lookup"><span data-stu-id="67bb3-162">Inside this delegate, the caller is expected to fill in the rest of the **TraceRecord**.</span></span>
3. <span data-ttu-id="67bb3-163">Scrivere il **TraceRecord**, usando tutte le tecniche che si desidera che la registrazione.</span><span class="sxs-lookup"><span data-stu-id="67bb3-163">Write the **TraceRecord**, using any logging technique that you like.</span></span> <span data-ttu-id="67bb3-164">Nell'esempio illustrato di seguito chiama semplicemente **Trace**.</span><span class="sxs-lookup"><span data-stu-id="67bb3-164">The example shown here simply calls into **System.Diagnostics.Trace**.</span></span>

## <a name="setting-the-trace-writer"></a><span data-ttu-id="67bb3-165">Impostare il Writer di traccia</span><span class="sxs-lookup"><span data-stu-id="67bb3-165">Setting the Trace Writer</span></span>

<span data-ttu-id="67bb3-166">Per abilitare la traccia, è necessario configurare Web API da usare il **ITraceWriter** implementazione.</span><span class="sxs-lookup"><span data-stu-id="67bb3-166">To enable tracing, you must configure Web API to use your **ITraceWriter** implementation.</span></span> <span data-ttu-id="67bb3-167">Eseguire questa operazione tramite il **HttpConfiguration** dell'oggetto, come illustrato nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="67bb3-167">You do this through the **HttpConfiguration** object, as shown in the following code:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="67bb3-168">Writer di traccia solo uno possono essere attivi.</span><span class="sxs-lookup"><span data-stu-id="67bb3-168">Only one trace writer can be active.</span></span> <span data-ttu-id="67bb3-169">Per impostazione predefinita, set di API Web un &quot;no-op&quot; traccia che non esegue alcuna operazione.</span><span class="sxs-lookup"><span data-stu-id="67bb3-169">By default, Web API sets a &quot;no-op&quot; tracer that does nothing.</span></span> <span data-ttu-id="67bb3-170">(Il &quot;no-op&quot; utilità di traccia esistente in modo che non deve controllare se il writer di traccia è il codice di tracciatura **null** prima della scrittura di una traccia.)</span><span class="sxs-lookup"><span data-stu-id="67bb3-170">(The &quot;no-op&quot; tracer exists so that tracing code does not have to check whether the trace writer is **null** before writing a trace.)</span></span>

## <a name="how-web-api-tracing-works"></a><span data-ttu-id="67bb3-171">Come Web API traccia Works</span><span class="sxs-lookup"><span data-stu-id="67bb3-171">How Web API Tracing Works</span></span>

<span data-ttu-id="67bb3-172">La traccia nell'API Web Usa un *facciata* pattern: quando la traccia è abilitata, API Web esegue il wrapping di diverse parti di pipeline della richiesta con le classi che eseguono chiamate di traccia.</span><span class="sxs-lookup"><span data-stu-id="67bb3-172">Tracing in Web API uses a *facade* pattern: When tracing is enabled, Web API wraps various parts of the request pipeline with classes that perform trace calls.</span></span>

<span data-ttu-id="67bb3-173">Ad esempio, quando si seleziona un controller, la pipeline Usa il **IHttpControllerSelector** interfaccia.</span><span class="sxs-lookup"><span data-stu-id="67bb3-173">For example, when selecting a controller, the pipeline uses the **IHttpControllerSelector** interface.</span></span> <span data-ttu-id="67bb3-174">Con la traccia abilitata, il pipleline inserisce una classe che implementa **IHttpControllerSelector** ma through delle chiamate per l'implementazione reale:</span><span class="sxs-lookup"><span data-stu-id="67bb3-174">With tracing enabled, the pipleline inserts a class that implements **IHttpControllerSelector** but calls through to the real implementation:</span></span>

![Traccia API Web Usa il modello di facciata.](tracing-in-aspnet-web-api/_static/image8.png)

<span data-ttu-id="67bb3-176">I vantaggi di questa progettazione includono:</span><span class="sxs-lookup"><span data-stu-id="67bb3-176">The benefits of this design include:</span></span>

- <span data-ttu-id="67bb3-177">Se non si aggiunge un writer di traccia, i componenti di traccia non vengono create istanze e non abbiano alcun impatto sulle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="67bb3-177">If you do not add a trace writer, the tracing components are not instantiated and have no performance impact.</span></span>
- <span data-ttu-id="67bb3-178">Se si sostituiscono servizi predefiniti, ad esempio **IHttpControllerSelector** con la propria implementazione personalizzata, la traccia non è interessata, perché la traccia viene eseguita dall'oggetto wrapper.</span><span class="sxs-lookup"><span data-stu-id="67bb3-178">If you replace default services such as **IHttpControllerSelector** with your own custom implementation, tracing is not affected, because tracing is done by the wrapper object.</span></span>

<span data-ttu-id="67bb3-179">È inoltre possibile sostituire l'intero framework traccia API Web con il proprio framework personalizzati, sostituendo il valore predefinito **ITraceManager** servizio:</span><span class="sxs-lookup"><span data-stu-id="67bb3-179">You can also replace the entire Web API trace framework with your own custom framework, by replacing the default **ITraceManager** service:</span></span>

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="67bb3-180">Implementare **ITraceManager.Initialize** per inizializzare il sistema di analisi.</span><span class="sxs-lookup"><span data-stu-id="67bb3-180">Implement **ITraceManager.Initialize** to initialize your tracing system.</span></span> <span data-ttu-id="67bb3-181">Tenere presente che questo sostituisce il *intera* framework di traccia, tra cui tutto il codice di traccia che è incorporato in API Web.</span><span class="sxs-lookup"><span data-stu-id="67bb3-181">Be aware that this replaces the *entire* trace framework, including all of the tracing code that is built into Web API.</span></span>
