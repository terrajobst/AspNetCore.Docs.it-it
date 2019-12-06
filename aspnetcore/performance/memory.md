---
title: Gestione della memoria e modelli in ASP.NET Core
author: rick-anderson
description: Informazioni sulla gestione della memoria in ASP.NET Core e sul funzionamento della Garbage Collector (GC).
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: performance/memory
ms.openlocfilehash: 85e34c9faa31a1020a4200eb99003455ca435ec3
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880944"
---
# <a name="memory-management-and-garbage-collection-gc-in-aspnet-core"></a><span data-ttu-id="c0656-103">Gestione della memoria e Garbage Collection (GC) in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c0656-103">Memory management and garbage collection (GC) in ASP.NET Core</span></span>

<span data-ttu-id="c0656-104">Di [Sébastien Ros](https://github.com/sebastienros) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c0656-104">By [Sébastien Ros](https://github.com/sebastienros) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c0656-105">La gestione della memoria è complessa, anche in un Framework gestito come .NET.</span><span class="sxs-lookup"><span data-stu-id="c0656-105">Memory management is complex, even in a managed framework like .NET.</span></span> <span data-ttu-id="c0656-106">L'analisi e la comprensione dei problemi di memoria possono essere complesse.</span><span class="sxs-lookup"><span data-stu-id="c0656-106">Analyzing and understanding memory issues can be challenging.</span></span> <span data-ttu-id="c0656-107">Questo articolo:</span><span class="sxs-lookup"><span data-stu-id="c0656-107">This article:</span></span>

* <span data-ttu-id="c0656-108">È stato motivato da molte *perdite di memoria* e da problemi *GC non funzionanti* .</span><span class="sxs-lookup"><span data-stu-id="c0656-108">Was motivated by many *memory leak* and *GC not working* issues.</span></span> <span data-ttu-id="c0656-109">La maggior parte di questi problemi è stata causata dalla mancata comprensione del funzionamento dell'utilizzo della memoria in .NET Core o dalla mancata comprensione del modo in cui viene misurata.</span><span class="sxs-lookup"><span data-stu-id="c0656-109">Most of these issues were caused by not understanding how memory consumption works in .NET Core, or not understanding how it's measured.</span></span>
* <span data-ttu-id="c0656-110">Viene illustrato l'utilizzo di memoria problematico e vengono suggeriti approcci alternativi.</span><span class="sxs-lookup"><span data-stu-id="c0656-110">Demonstrates problematic memory use, and suggests alternative approaches.</span></span>

## <a name="how-garbage-collection-gc-works-in-net-core"></a><span data-ttu-id="c0656-111">Funzionamento di Garbage Collection (GC) in .NET Core</span><span class="sxs-lookup"><span data-stu-id="c0656-111">How garbage collection (GC) works in .NET Core</span></span>

<span data-ttu-id="c0656-112">Il GC alloca i segmenti dell'heap in cui ogni segmento è un intervallo di memoria contiguo.</span><span class="sxs-lookup"><span data-stu-id="c0656-112">The GC allocates heap segments where each segment is a contiguous range of memory.</span></span> <span data-ttu-id="c0656-113">Gli oggetti inseriti nell'heap vengono categorizzati in una delle tre generazioni seguenti: 0, 1 o 2.</span><span class="sxs-lookup"><span data-stu-id="c0656-113">Objects placed in the heap are categorized into one of 3 generations: 0, 1, or 2.</span></span> <span data-ttu-id="c0656-114">La generazione determina la frequenza con cui il GC tenta di rilasciare la memoria sugli oggetti gestiti a cui l'app non fa più riferimento.</span><span class="sxs-lookup"><span data-stu-id="c0656-114">The generation determines the frequency the GC attempts to release memory on managed objects that are no longer referenced by the app.</span></span> <span data-ttu-id="c0656-115">Le generazioni con numero inferiore sono più frequenti.</span><span class="sxs-lookup"><span data-stu-id="c0656-115">Lower numbered generations are GC'd more frequently.</span></span>

<span data-ttu-id="c0656-116">Gli oggetti vengono spostati da una generazione all'altra in base al relativo ciclo di vita.</span><span class="sxs-lookup"><span data-stu-id="c0656-116">Objects are moved from one generation to another based on their lifetime.</span></span> <span data-ttu-id="c0656-117">Man mano che gli oggetti si trovano più a lungo, vengono spostati in una generazione più elevata.</span><span class="sxs-lookup"><span data-stu-id="c0656-117">As objects live longer, they are moved into a higher generation.</span></span> <span data-ttu-id="c0656-118">Come indicato in precedenza, le generazioni più elevate sono meno frequenti.</span><span class="sxs-lookup"><span data-stu-id="c0656-118">As mentioned previously, higher generations are GC'd less often.</span></span> <span data-ttu-id="c0656-119">Gli oggetti vissuti a breve termine rimangono sempre nella generazione 0.</span><span class="sxs-lookup"><span data-stu-id="c0656-119">Short term lived objects always remain in generation 0.</span></span> <span data-ttu-id="c0656-120">Ad esempio, gli oggetti a cui viene fatto riferimento durante il ciclo di vita di una richiesta Web sono di breve durata.</span><span class="sxs-lookup"><span data-stu-id="c0656-120">For example, objects that are referenced during the life of a web request are short lived.</span></span> <span data-ttu-id="c0656-121">I [singleton](xref:fundamentals/dependency-injection#service-lifetimes) a livello di applicazione in genere migrano alla generazione 2.</span><span class="sxs-lookup"><span data-stu-id="c0656-121">Application level [singletons](xref:fundamentals/dependency-injection#service-lifetimes) generally migrate to generation 2.</span></span>

<span data-ttu-id="c0656-122">Quando viene avviata un'app ASP.NET Core, il GC:</span><span class="sxs-lookup"><span data-stu-id="c0656-122">When an ASP.NET Core app starts, the GC:</span></span>

* <span data-ttu-id="c0656-123">Riserva una certa quantità di memoria per i segmenti heap iniziali.</span><span class="sxs-lookup"><span data-stu-id="c0656-123">Reserves some memory for the initial heap segments.</span></span>
* <span data-ttu-id="c0656-124">Esegue il commit di una piccola parte della memoria quando il runtime viene caricato.</span><span class="sxs-lookup"><span data-stu-id="c0656-124">Commits a small portion of memory when the runtime is loaded.</span></span>

<span data-ttu-id="c0656-125">Le allocazioni di memoria precedenti vengono eseguite per motivi di prestazioni.</span><span class="sxs-lookup"><span data-stu-id="c0656-125">The preceding memory allocations are done for performance reasons.</span></span> <span data-ttu-id="c0656-126">Il vantaggio in termini di prestazioni deriva da segmenti di heap nella memoria contigua.</span><span class="sxs-lookup"><span data-stu-id="c0656-126">The performance benefit comes from heap segments in contiguous memory.</span></span>

### <a name="call-gccollect"></a><span data-ttu-id="c0656-127">Chiamare GC. Raccogliere</span><span class="sxs-lookup"><span data-stu-id="c0656-127">Call GC.Collect</span></span>

<span data-ttu-id="c0656-128">Chiamata a [GC. Raccogli](xref:System.GC.Collect*) in modo esplicito:</span><span class="sxs-lookup"><span data-stu-id="c0656-128">Calling [GC.Collect](xref:System.GC.Collect*) explicitly:</span></span>

* <span data-ttu-id="c0656-129">**Non** deve essere eseguita dalle app ASP.NET Core di produzione.</span><span class="sxs-lookup"><span data-stu-id="c0656-129">Should **not** be done by production ASP.NET Core apps.</span></span>
* <span data-ttu-id="c0656-130">È utile quando si esaminano le perdite di memoria.</span><span class="sxs-lookup"><span data-stu-id="c0656-130">Is useful when investigating memory leaks.</span></span>
* <span data-ttu-id="c0656-131">Quando si esamina, verifica che il GC abbia rimosso tutti gli oggetti penzolanti dalla memoria, in modo che sia possibile misurare la memoria.</span><span class="sxs-lookup"><span data-stu-id="c0656-131">When investigating, verifies the GC has removed all dangling objects from memory so memory can be measured.</span></span>

## <a name="analyzing-the-memory-usage-of-an-app"></a><span data-ttu-id="c0656-132">Analisi dell'utilizzo della memoria di un'app</span><span class="sxs-lookup"><span data-stu-id="c0656-132">Analyzing the memory usage of an app</span></span>

<span data-ttu-id="c0656-133">Gli strumenti dedicati consentono di analizzare l'utilizzo della memoria:</span><span class="sxs-lookup"><span data-stu-id="c0656-133">Dedicated tools can help analyzing memory usage:</span></span>

- <span data-ttu-id="c0656-134">Conteggio dei riferimenti a oggetti</span><span class="sxs-lookup"><span data-stu-id="c0656-134">Counting object references</span></span>
- <span data-ttu-id="c0656-135">Misurazione dell'effetto del Garbage Collector sull'utilizzo della CPU</span><span class="sxs-lookup"><span data-stu-id="c0656-135">Measuring how much impact the GC has on CPU usage</span></span>
- <span data-ttu-id="c0656-136">Misurazione dello spazio di memoria usato per ogni generazione</span><span class="sxs-lookup"><span data-stu-id="c0656-136">Measuring memory space used for each generation</span></span>

<span data-ttu-id="c0656-137">Usare gli strumenti seguenti per analizzare l'utilizzo della memoria:</span><span class="sxs-lookup"><span data-stu-id="c0656-137">Use the following tools to analyze memory usage:</span></span>

* <span data-ttu-id="c0656-138">[DotNet-Trace](/dotnet/core/diagnostics/dotnet-trace): può essere usato nei computer di produzione.</span><span class="sxs-lookup"><span data-stu-id="c0656-138">[dotnet-trace](/dotnet/core/diagnostics/dotnet-trace): Can be  used on production machines.</span></span>
* [<span data-ttu-id="c0656-139">Analizzare l'utilizzo della memoria senza il debugger di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c0656-139">Analyze memory usage without the Visual Studio debugger</span></span>](/visualstudio/profiling/memory-usage-without-debugging2)
* [<span data-ttu-id="c0656-140">Profilare l'utilizzo della memoria in Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c0656-140">Profile memory usage in Visual Studio</span></span>](/visualstudio/profiling/memory-usage)

### <a name="detecting-memory-issues"></a><span data-ttu-id="c0656-141">Rilevamento dei problemi di memoria</span><span class="sxs-lookup"><span data-stu-id="c0656-141">Detecting memory issues</span></span>

<span data-ttu-id="c0656-142">È possibile usare Gestione attività per avere un'idea della quantità di memoria usata da un'app ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c0656-142">Task Manager can be used to get an idea of how much memory an ASP.NET app is using.</span></span> <span data-ttu-id="c0656-143">Valore di memoria di gestione attività:</span><span class="sxs-lookup"><span data-stu-id="c0656-143">The Task Manager memory value:</span></span>

* <span data-ttu-id="c0656-144">Rappresenta la quantità di memoria utilizzata dal processo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c0656-144">Represents the amount of memory that is used by the ASP.NET process.</span></span>
* <span data-ttu-id="c0656-145">Include gli oggetti Living dell'app e altri consumer di memoria, ad esempio l'utilizzo della memoria nativa.</span><span class="sxs-lookup"><span data-stu-id="c0656-145">Includes the app's living objects and other memory consumers such as native memory usage.</span></span>

<span data-ttu-id="c0656-146">Se il valore di memoria di gestione attività aumenta a tempo indefinito e non si rende mai Flat, l'app presenta una perdita di memoria.</span><span class="sxs-lookup"><span data-stu-id="c0656-146">If the Task Manager memory value increases indefinitely and never flattens out, the app has a memory leak.</span></span> <span data-ttu-id="c0656-147">Le sezioni seguenti illustrano e spiegano diversi modelli di utilizzo della memoria.</span><span class="sxs-lookup"><span data-stu-id="c0656-147">The following sections demonstrate and explain several memory usage patterns.</span></span>

## <a name="sample-display-memory-usage-app"></a><span data-ttu-id="c0656-148">App di esempio per visualizzare l'utilizzo della memoria</span><span class="sxs-lookup"><span data-stu-id="c0656-148">Sample display memory usage app</span></span>

<span data-ttu-id="c0656-149">L' [app di esempio Memoryleak](https://github.com/sebastienros/memoryleak) è disponibile in GitHub.</span><span class="sxs-lookup"><span data-stu-id="c0656-149">The [MemoryLeak sample app](https://github.com/sebastienros/memoryleak) is available on GitHub.</span></span> <span data-ttu-id="c0656-150">App MemoryLeak:</span><span class="sxs-lookup"><span data-stu-id="c0656-150">The MemoryLeak app:</span></span>

* <span data-ttu-id="c0656-151">Include un controller di diagnostica che raccoglie la memoria in tempo reale e i dati GC per l'app.</span><span class="sxs-lookup"><span data-stu-id="c0656-151">Includes a diagnostic controller that gathers real-time memory and GC data for the app.</span></span>
* <span data-ttu-id="c0656-152">Dispone di una pagina di indice che Visualizza i dati di memoria e GC.</span><span class="sxs-lookup"><span data-stu-id="c0656-152">Has an Index page that displays the memory and GC data.</span></span> <span data-ttu-id="c0656-153">La pagina di indice viene aggiornata ogni secondo.</span><span class="sxs-lookup"><span data-stu-id="c0656-153">The Index page is refreshed every second.</span></span>
* <span data-ttu-id="c0656-154">Contiene un controller API che fornisce diversi modelli di carico di memoria.</span><span class="sxs-lookup"><span data-stu-id="c0656-154">Contains an API controller that provides various memory load patterns.</span></span>
* <span data-ttu-id="c0656-155">Non è uno strumento supportato, tuttavia, può essere usato per visualizzare i modelli di utilizzo della memoria delle app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c0656-155">Is not a supported tool, however, it can be used to display memory usage patterns of ASP.NET Core apps.</span></span>

<span data-ttu-id="c0656-156">Eseguire MemoryLeak.</span><span class="sxs-lookup"><span data-stu-id="c0656-156">Run MemoryLeak.</span></span> <span data-ttu-id="c0656-157">La memoria allocata aumenta lentamente fino a quando non si verifica un catalogo globale.</span><span class="sxs-lookup"><span data-stu-id="c0656-157">Allocated memory slowly increases until a GC occurs.</span></span> <span data-ttu-id="c0656-158">La memoria aumenta perché lo strumento alloca l'oggetto personalizzato per l'acquisizione dei dati.</span><span class="sxs-lookup"><span data-stu-id="c0656-158">Memory increases because the tool allocates custom object to capture data.</span></span> <span data-ttu-id="c0656-159">La figura seguente mostra la pagina di indice MemoryLeak quando si verifica un GC di generazione 0.</span><span class="sxs-lookup"><span data-stu-id="c0656-159">The following image shows the MemoryLeak Index page when a Gen 0 GC occurs.</span></span> <span data-ttu-id="c0656-160">Il grafico Mostra 0 RPS (richieste al secondo) perché non è stato chiamato alcun endpoint API dal controller API.</span><span class="sxs-lookup"><span data-stu-id="c0656-160">The chart shows 0 RPS (Requests per second) because no API endpoints from the API controller have been called.</span></span>

![grafico precedente](memory/_static/0RPS.png)

<span data-ttu-id="c0656-162">Nel grafico vengono visualizzati due valori per l'utilizzo della memoria:</span><span class="sxs-lookup"><span data-stu-id="c0656-162">The chart displays two values for the memory usage:</span></span>

- <span data-ttu-id="c0656-163">Allocata: quantità di memoria occupata dagli oggetti gestiti</span><span class="sxs-lookup"><span data-stu-id="c0656-163">Allocated: the amount of memory occupied by managed objects</span></span>
- <span data-ttu-id="c0656-164">[Working set](/windows/win32/memory/working-set): set di pagine nello spazio degli indirizzi virtuali del processo che attualmente risiede nella memoria fisica.</span><span class="sxs-lookup"><span data-stu-id="c0656-164">[Working set](/windows/win32/memory/working-set): The set of pages in the virtual address space of the process that are currently resident in physical memory.</span></span> <span data-ttu-id="c0656-165">Il working set visualizzato è lo stesso valore visualizzato in Gestione attività.</span><span class="sxs-lookup"><span data-stu-id="c0656-165">The working set shown is the same value Task Manager displays.</span></span>

### <a name="transient-objects"></a><span data-ttu-id="c0656-166">Oggetti temporanei</span><span class="sxs-lookup"><span data-stu-id="c0656-166">Transient objects</span></span>

<span data-ttu-id="c0656-167">L'API seguente crea un'istanza di stringa di 10 KB e la restituisce al client.</span><span class="sxs-lookup"><span data-stu-id="c0656-167">The following API creates a 10-KB String instance and returns it to the client.</span></span> <span data-ttu-id="c0656-168">Per ogni richiesta, un nuovo oggetto viene allocato in memoria e scritto nella risposta.</span><span class="sxs-lookup"><span data-stu-id="c0656-168">On each request, a new object is allocated in memory and written to the response.</span></span> <span data-ttu-id="c0656-169">Le stringhe vengono archiviate come caratteri UTF-16 in .NET in modo che ogni carattere accetti 2 byte di memoria.</span><span class="sxs-lookup"><span data-stu-id="c0656-169">Strings are stored as UTF-16 characters in .NET so each character takes 2 bytes in memory.</span></span>

```csharp
[HttpGet("bigstring")]
public ActionResult<string> GetBigString()
{
    return new String('x', 10 * 1024);
}
```

<span data-ttu-id="c0656-170">Il grafico seguente viene generato con un carico relativamente piccolo in per illustrare il modo in cui le allocazioni di memoria sono interessate dal GC.</span><span class="sxs-lookup"><span data-stu-id="c0656-170">The following graph is generated with a relatively small load in to show how memory allocations are impacted by the GC.</span></span>

![grafico precedente](memory/_static/bigstring.png)

<span data-ttu-id="c0656-172">Il grafico precedente Mostra:</span><span class="sxs-lookup"><span data-stu-id="c0656-172">The preceding chart shows:</span></span>

* <span data-ttu-id="c0656-173">RPS 4K (richieste al secondo).</span><span class="sxs-lookup"><span data-stu-id="c0656-173">4K RPS (Requests per second).</span></span>
* <span data-ttu-id="c0656-174">Le raccolte GC di generazione 0 si verificano ogni due secondi.</span><span class="sxs-lookup"><span data-stu-id="c0656-174">Generation 0 GC collections occur about every two seconds.</span></span>
* <span data-ttu-id="c0656-175">Il working set è costante a circa 500 MB.</span><span class="sxs-lookup"><span data-stu-id="c0656-175">The working set is constant at approximately 500 MB.</span></span>
* <span data-ttu-id="c0656-176">CPU è il 12%.</span><span class="sxs-lookup"><span data-stu-id="c0656-176">CPU is 12%.</span></span>
* <span data-ttu-id="c0656-177">Il consumo di memoria e la versione (tramite GC) è stabile.</span><span class="sxs-lookup"><span data-stu-id="c0656-177">The memory consumption and release (through GC) is stable.</span></span>

<span data-ttu-id="c0656-178">Il grafico seguente viene considerato alla velocità effettiva massima che può essere gestita dal computer.</span><span class="sxs-lookup"><span data-stu-id="c0656-178">The following chart is taken at the max throughput that can be handled by the machine.</span></span>

![grafico precedente](memory/_static/bigstring2.png)

<span data-ttu-id="c0656-180">Il grafico precedente Mostra:</span><span class="sxs-lookup"><span data-stu-id="c0656-180">The preceding chart shows:</span></span>

* <span data-ttu-id="c0656-181">RPS 22K</span><span class="sxs-lookup"><span data-stu-id="c0656-181">22K RPS</span></span>
* <span data-ttu-id="c0656-182">Le raccolte GC di generazione 0 si verificano più volte al secondo.</span><span class="sxs-lookup"><span data-stu-id="c0656-182">Generation 0 GC collections occur several times per second.</span></span>
* <span data-ttu-id="c0656-183">Le raccolte di generazione 1 vengono attivate perché l'app ha allocato significativamente più memoria al secondo.</span><span class="sxs-lookup"><span data-stu-id="c0656-183">Generation 1 collections are triggered because the app allocated significantly more memory per second.</span></span>
* <span data-ttu-id="c0656-184">Il working set è costante a circa 500 MB.</span><span class="sxs-lookup"><span data-stu-id="c0656-184">The working set is constant at approximately 500 MB.</span></span>
* <span data-ttu-id="c0656-185">La CPU è pari al 33%.</span><span class="sxs-lookup"><span data-stu-id="c0656-185">CPU is 33%.</span></span>
* <span data-ttu-id="c0656-186">Il consumo di memoria e la versione (tramite GC) è stabile.</span><span class="sxs-lookup"><span data-stu-id="c0656-186">The memory consumption and release (through GC) is stable.</span></span>
* <span data-ttu-id="c0656-187">CPU (33%) non è sovrautilizzato, quindi il Garbage Collection può essere aggiornato con un numero elevato di allocazioni.</span><span class="sxs-lookup"><span data-stu-id="c0656-187">The CPU (33%) is not over-utilized, therefore the garbage collection can keep up with a high number of allocations.</span></span>

### <a name="workstation-gc-vs-server-gc"></a><span data-ttu-id="c0656-188">GC workstation rispetto a GC server</span><span class="sxs-lookup"><span data-stu-id="c0656-188">Workstation GC vs. Server GC</span></span>

<span data-ttu-id="c0656-189">Il Garbage Collector di .NET prevede due modalità diverse:</span><span class="sxs-lookup"><span data-stu-id="c0656-189">The .NET Garbage Collector has two different modes:</span></span>

* <span data-ttu-id="c0656-190">**GC workstation**: ottimizzato per il desktop.</span><span class="sxs-lookup"><span data-stu-id="c0656-190">**Workstation GC**: Optimized for the desktop.</span></span>
* <span data-ttu-id="c0656-191">**GC server**.</span><span class="sxs-lookup"><span data-stu-id="c0656-191">**Server GC**.</span></span> <span data-ttu-id="c0656-192">GC predefinito per le app ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c0656-192">The default GC for ASP.NET Core apps.</span></span> <span data-ttu-id="c0656-193">Ottimizzato per il server.</span><span class="sxs-lookup"><span data-stu-id="c0656-193">Optimized for the server.</span></span>

<span data-ttu-id="c0656-194">La modalità GC può essere impostata in modo esplicito nel file di progetto o nel file *runtimeconfig. JSON* dell'app pubblicata.</span><span class="sxs-lookup"><span data-stu-id="c0656-194">The GC mode can be set explicitly in the project file or in the *runtimeconfig.json* file of the published app.</span></span> <span data-ttu-id="c0656-195">Il markup seguente mostra l'impostazione di `ServerGarbageCollection` nel file di progetto:</span><span class="sxs-lookup"><span data-stu-id="c0656-195">The following markup shows setting `ServerGarbageCollection` in the project file:</span></span>

```xml
<PropertyGroup>
  <ServerGarbageCollection>true</ServerGarbageCollection>
</PropertyGroup>
```

<span data-ttu-id="c0656-196">Per modificare `ServerGarbageCollection` nel file di progetto, è necessario ricompilare l'app.</span><span class="sxs-lookup"><span data-stu-id="c0656-196">Changing `ServerGarbageCollection` in the project file requires the app to be rebuilt.</span></span>

<span data-ttu-id="c0656-197">**Nota:** Il Garbage Collection server **non** è disponibile nei computer con un singolo core.</span><span class="sxs-lookup"><span data-stu-id="c0656-197">**Note:** Server garbage collection is **not** available on machines with a single core.</span></span> <span data-ttu-id="c0656-198">Per ulteriori informazioni, vedere <xref:System.Runtime.GCSettings.IsServerGC>.</span><span class="sxs-lookup"><span data-stu-id="c0656-198">For more information, see <xref:System.Runtime.GCSettings.IsServerGC>.</span></span>

<span data-ttu-id="c0656-199">La figura seguente mostra il profilo di memoria in un RPS da 5K usando il GC della workstation.</span><span class="sxs-lookup"><span data-stu-id="c0656-199">The following image shows the memory profile under a 5K RPS using the Workstation GC.</span></span>

![grafico precedente](memory/_static/workstation.png)

<span data-ttu-id="c0656-201">Le differenze tra il grafico e la versione del server sono significative:</span><span class="sxs-lookup"><span data-stu-id="c0656-201">The differences between this chart and the server version are significant:</span></span>

- <span data-ttu-id="c0656-202">Il working set scende da 500 MB a 70 MB.</span><span class="sxs-lookup"><span data-stu-id="c0656-202">The working set drops from 500 MB to 70 MB.</span></span>
- <span data-ttu-id="c0656-203">Il GC esegue raccolte di generazione 0 più volte al secondo anziché ogni due secondi.</span><span class="sxs-lookup"><span data-stu-id="c0656-203">The GC does generation 0 collections multiple times per second instead of every two seconds.</span></span>
- <span data-ttu-id="c0656-204">GC scende da 300 MB a 10 MB.</span><span class="sxs-lookup"><span data-stu-id="c0656-204">GC drops from 300 MB to 10 MB.</span></span>

<span data-ttu-id="c0656-205">In un tipico ambiente server Web, l'utilizzo della CPU è più importante della memoria, quindi il GC del server è migliore.</span><span class="sxs-lookup"><span data-stu-id="c0656-205">On a typical web server environment, CPU usage is more important than memory, therefore the Server GC is better.</span></span> <span data-ttu-id="c0656-206">Se l'utilizzo della memoria è elevato e l'utilizzo della CPU è relativamente basso, il GC della workstation potrebbe essere più efficiente.</span><span class="sxs-lookup"><span data-stu-id="c0656-206">If memory utilization is high and CPU usage is relatively low, the Workstation GC might be more performant.</span></span> <span data-ttu-id="c0656-207">Ad esempio, la densità elevata ospita diverse app Web in cui la memoria è limitata.</span><span class="sxs-lookup"><span data-stu-id="c0656-207">For example, high density hosting several web apps where memory is scarce.</span></span>

### <a name="persistent-object-references"></a><span data-ttu-id="c0656-208">Riferimenti a oggetti permanenti</span><span class="sxs-lookup"><span data-stu-id="c0656-208">Persistent object references</span></span>

<span data-ttu-id="c0656-209">Il Garbage Collector non può liberare oggetti a cui si fa riferimento.</span><span class="sxs-lookup"><span data-stu-id="c0656-209">The GC cannot free objects that are referenced.</span></span> <span data-ttu-id="c0656-210">Gli oggetti a cui viene fatto riferimento ma che non sono più necessari generano una perdita di memoria.</span><span class="sxs-lookup"><span data-stu-id="c0656-210">Objects that are referenced but no longer needed result in a memory leak.</span></span> <span data-ttu-id="c0656-211">Se l'app alloca spesso oggetti e non riesce a liberarli dopo che non sono più necessari, l'utilizzo della memoria aumenterà nel tempo.</span><span class="sxs-lookup"><span data-stu-id="c0656-211">If the app frequently allocates objects and fails to free them after they are no longer needed, memory usage will increase over time.</span></span>

<span data-ttu-id="c0656-212">L'API seguente crea un'istanza di stringa di 10 KB e la restituisce al client.</span><span class="sxs-lookup"><span data-stu-id="c0656-212">The following API creates a 10-KB String instance and returns it to the client.</span></span> <span data-ttu-id="c0656-213">La differenza con l'esempio precedente è che a questa istanza viene fatto riferimento da un membro statico, il che significa che non è mai disponibile per la raccolta.</span><span class="sxs-lookup"><span data-stu-id="c0656-213">The difference with the previous example is that this instance is referenced by a static member, which means it's never available for collection.</span></span>

```csharp
private static ConcurrentBag<string> _staticStrings = new ConcurrentBag<string>();

[HttpGet("staticstring")]
public ActionResult<string> GetStaticString()
{
    var bigString = new String('x', 10 * 1024);
    _staticStrings.Add(bigString);
    return bigString;
}
```

<span data-ttu-id="c0656-214">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="c0656-214">The preceding code:</span></span>

* <span data-ttu-id="c0656-215">È un esempio di una perdita di memoria tipica.</span><span class="sxs-lookup"><span data-stu-id="c0656-215">Is an example of a typical memory leak.</span></span>
* <span data-ttu-id="c0656-216">Con chiamate frequenti, causa l'aumento della memoria dell'app finché il processo non si arresta in modo anomalo con un'eccezione `OutOfMemory`.</span><span class="sxs-lookup"><span data-stu-id="c0656-216">With frequent calls, causes app memory to increases until the process crashes with an `OutOfMemory` exception.</span></span>

![grafico precedente](memory/_static/eternal.png)

<span data-ttu-id="c0656-218">Nell'immagine precedente:</span><span class="sxs-lookup"><span data-stu-id="c0656-218">In the preceding image:</span></span>

* <span data-ttu-id="c0656-219">Il test di carico l'endpoint `/api/staticstring` causa un aumento lineare della memoria.</span><span class="sxs-lookup"><span data-stu-id="c0656-219">Load testing the `/api/staticstring` endpoint causes a linear increase in memory.</span></span>
* <span data-ttu-id="c0656-220">Il GC tenta di liberare memoria man mano che aumenta la quantità di memoria, chiamando una raccolta di generazione 2.</span><span class="sxs-lookup"><span data-stu-id="c0656-220">The GC tries to free memory as the memory pressure grows, by calling a generation 2 collection.</span></span>
* <span data-ttu-id="c0656-221">Il Garbage Collector non può liberare la memoria persa.</span><span class="sxs-lookup"><span data-stu-id="c0656-221">The GC cannot free the leaked memory.</span></span> <span data-ttu-id="c0656-222">Allocato e working set aumentare con il tempo.</span><span class="sxs-lookup"><span data-stu-id="c0656-222">Allocated and working set increase with time.</span></span>

<span data-ttu-id="c0656-223">Per alcuni scenari, ad esempio la memorizzazione nella cache, è necessario che i riferimenti agli oggetti vengano mantenuti fino a quando non viene forzato il rilascio della memoria.</span><span class="sxs-lookup"><span data-stu-id="c0656-223">Some scenarios, such as caching, require object references to be held until memory pressure forces them to be released.</span></span> <span data-ttu-id="c0656-224">La classe <xref:System.WeakReference> può essere utilizzata per questo tipo di codice di memorizzazione nella cache.</span><span class="sxs-lookup"><span data-stu-id="c0656-224">The <xref:System.WeakReference> class can be used for this type of caching code.</span></span> <span data-ttu-id="c0656-225">Un oggetto `WeakReference` viene raccolto in un numero elevato di richieste di memoria.</span><span class="sxs-lookup"><span data-stu-id="c0656-225">A `WeakReference` object is collected under memory pressures.</span></span> <span data-ttu-id="c0656-226">L'implementazione predefinita di <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache> USA `WeakReference`.</span><span class="sxs-lookup"><span data-stu-id="c0656-226">The default implementation of <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache> uses `WeakReference`.</span></span>

### <a name="native-memory"></a><span data-ttu-id="c0656-227">Memoria nativa</span><span class="sxs-lookup"><span data-stu-id="c0656-227">Native memory</span></span>

<span data-ttu-id="c0656-228">Alcuni oggetti .NET Core si basano sulla memoria nativa.</span><span class="sxs-lookup"><span data-stu-id="c0656-228">Some .NET Core objects rely on native memory.</span></span> <span data-ttu-id="c0656-229">La memoria nativa **non** può essere raccolta dal GC.</span><span class="sxs-lookup"><span data-stu-id="c0656-229">Native memory can **not** be collected by the GC.</span></span> <span data-ttu-id="c0656-230">L'oggetto .NET che utilizza la memoria nativa deve liberarlo utilizzando codice nativo.</span><span class="sxs-lookup"><span data-stu-id="c0656-230">The .NET object using native memory must free it using native code.</span></span>

<span data-ttu-id="c0656-231">.NET fornisce l'interfaccia <xref:System.IDisposable> per consentire agli sviluppatori di rilasciare la memoria nativa.</span><span class="sxs-lookup"><span data-stu-id="c0656-231">.NET provides the <xref:System.IDisposable> interface to let developers release native memory.</span></span> <span data-ttu-id="c0656-232">Anche se <xref:System.IDisposable.Dispose*> non viene chiamato, le classi implementate correttamente chiamano `Dispose` quando viene eseguito il [finalizzatore](/dotnet/csharp/programming-guide/classes-and-structs/destructors) .</span><span class="sxs-lookup"><span data-stu-id="c0656-232">Even if <xref:System.IDisposable.Dispose*> is not called, correctly implemented classes call `Dispose` when the [finalizer](/dotnet/csharp/programming-guide/classes-and-structs/destructors) runs.</span></span>

<span data-ttu-id="c0656-233">Esaminare il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="c0656-233">Consider the following code:</span></span>

```csharp
[HttpGet("fileprovider")]
public void GetFileProvider()
{
    var fp = new PhysicalFileProvider(TempPath);
    fp.Watch("*.*");
}
```

<span data-ttu-id="c0656-234">[PhysicaFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider?view=dotnet-plat-ext-3.0) è una classe gestita, quindi qualsiasi istanza verrà raccolta alla fine della richiesta.</span><span class="sxs-lookup"><span data-stu-id="c0656-234">[PhysicaFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider?view=dotnet-plat-ext-3.0) is a managed class, so any instance will be collected at the end of the request.</span></span>

<span data-ttu-id="c0656-235">La figura seguente mostra il profilo di memoria durante la chiamata continua dell'API `fileprovider`.</span><span class="sxs-lookup"><span data-stu-id="c0656-235">The following image shows the memory profile while invoking the `fileprovider` API continuously.</span></span>

![grafico precedente](memory/_static/fileprovider.png)

<span data-ttu-id="c0656-237">Il grafico precedente mostra un problema evidente con l'implementazione di questa classe, poiché continua ad aumentare l'utilizzo della memoria.</span><span class="sxs-lookup"><span data-stu-id="c0656-237">The preceding chart shows an obvious issue with the implementation of this class, as it keeps increasing memory usage.</span></span> <span data-ttu-id="c0656-238">Si tratta di un problema noto che viene rilevato in [questo problema](https://github.com/aspnet/Home/issues/3110).</span><span class="sxs-lookup"><span data-stu-id="c0656-238">This is a known problem that is being tracked in [this issue](https://github.com/aspnet/Home/issues/3110).</span></span>

<span data-ttu-id="c0656-239">La stessa perdita può verificarsi nel codice utente, in uno dei seguenti:</span><span class="sxs-lookup"><span data-stu-id="c0656-239">The same leak could be happen in user code, by one of the following:</span></span>

* <span data-ttu-id="c0656-240">Non è stata rilasciata correttamente la classe.</span><span class="sxs-lookup"><span data-stu-id="c0656-240">Not releasing the class correctly.</span></span>
* <span data-ttu-id="c0656-241">Si dimentica di richiamare il metodo `Dispose`degli oggetti dipendenti che devono essere eliminati.</span><span class="sxs-lookup"><span data-stu-id="c0656-241">Forgetting to invoke the `Dispose`method of the dependent objects that should be disposed.</span></span>

### <a name="large-objects-heap"></a><span data-ttu-id="c0656-242">Heap oggetti grandi</span><span class="sxs-lookup"><span data-stu-id="c0656-242">Large objects heap</span></span>

<span data-ttu-id="c0656-243">I cicli di allocazione di memoria/disponibili frequenti possono frammentare la memoria, soprattutto quando si allocano blocchi di memoria di grandi dimensioni.</span><span class="sxs-lookup"><span data-stu-id="c0656-243">Frequent memory allocation/free cycles can fragment memory, especially when allocating large chunks of memory.</span></span> <span data-ttu-id="c0656-244">Gli oggetti vengono allocati in blocchi di memoria contigui.</span><span class="sxs-lookup"><span data-stu-id="c0656-244">Objects are allocated in contiguous blocks of memory.</span></span> <span data-ttu-id="c0656-245">Per attenuare la frammentazione, quando il GC libera memoria, trys la deframmentazione.</span><span class="sxs-lookup"><span data-stu-id="c0656-245">To mitigate fragmentation, when the GC frees memory, it trys to defragment it.</span></span> <span data-ttu-id="c0656-246">Questo processo è denominato **compattazione**.</span><span class="sxs-lookup"><span data-stu-id="c0656-246">This process is called **compaction**.</span></span> <span data-ttu-id="c0656-247">La compattazione comporta lo stato di trasferimento degli oggetti.</span><span class="sxs-lookup"><span data-stu-id="c0656-247">Compaction involves moving objects.</span></span> <span data-ttu-id="c0656-248">Lo spostato di oggetti di grandi dimensioni impone una riduzione delle prestazioni.</span><span class="sxs-lookup"><span data-stu-id="c0656-248">Moving large objects imposes a performance penalty.</span></span> <span data-ttu-id="c0656-249">Per questo motivo il GC crea una zona di memoria speciale per oggetti di _grandi dimensioni_ , denominata [heap degli oggetti grandi](/dotnet/standard/garbage-collection/large-object-heap) (LOH).</span><span class="sxs-lookup"><span data-stu-id="c0656-249">For this reason the GC creates a special memory zone for _large_ objects, called the [large object heap](/dotnet/standard/garbage-collection/large-object-heap) (LOH).</span></span> <span data-ttu-id="c0656-250">Gli oggetti che sono maggiori di 85.000 byte (circa 83 KB) sono:</span><span class="sxs-lookup"><span data-stu-id="c0656-250">Objects that are greater than 85,000 bytes (approximately 83 KB) are:</span></span>

* <span data-ttu-id="c0656-251">Inserito nell'heap oggetti grandi.</span><span class="sxs-lookup"><span data-stu-id="c0656-251">Placed on the LOH.</span></span>
* <span data-ttu-id="c0656-252">Non compattato.</span><span class="sxs-lookup"><span data-stu-id="c0656-252">Not compacted.</span></span>
* <span data-ttu-id="c0656-253">Raccolta durante i cataloghi globali di generazione 2.</span><span class="sxs-lookup"><span data-stu-id="c0656-253">Collected during generation 2 GCs.</span></span>

<span data-ttu-id="c0656-254">Quando l'heap oggetti grandi è pieno, il GC attiverà una raccolta di generazione 2.</span><span class="sxs-lookup"><span data-stu-id="c0656-254">When the LOH is full, the GC will trigger a generation 2 collection.</span></span> <span data-ttu-id="c0656-255">Raccolte di generazione 2:</span><span class="sxs-lookup"><span data-stu-id="c0656-255">Generation 2 collections:</span></span>

* <span data-ttu-id="c0656-256">Sono intrinsecamente lenti.</span><span class="sxs-lookup"><span data-stu-id="c0656-256">Are inherently slow.</span></span>
* <span data-ttu-id="c0656-257">Comportano inoltre il costo di attivazione di una raccolta in tutte le altre generazioni.</span><span class="sxs-lookup"><span data-stu-id="c0656-257">Additionally incur the cost of triggering a collection on all other generations.</span></span>

<span data-ttu-id="c0656-258">Il codice seguente comprime immediatamente l'oggetto LOH:</span><span class="sxs-lookup"><span data-stu-id="c0656-258">The following code compacts the LOH immediately:</span></span>

```csharp
GCSettings.LargeObjectHeapCompactionMode = GCLargeObjectHeapCompactionMode.CompactOnce;
GC.Collect();
```

<span data-ttu-id="c0656-259">Per informazioni sulla compattazione dell'heap oggetti grandi, vedere <xref:System.Runtime.GCSettings.LargeObjectHeapCompactionMode>.</span><span class="sxs-lookup"><span data-stu-id="c0656-259">See <xref:System.Runtime.GCSettings.LargeObjectHeapCompactionMode> for information on compacting the LOH.</span></span>

<span data-ttu-id="c0656-260">Nei contenitori che usano .NET Core 3,0 e versioni successive, l'heap oggetti grandi viene compattato automaticamente.</span><span class="sxs-lookup"><span data-stu-id="c0656-260">In containers using .NET Core 3.0 and later, the LOH is automatically compacted.</span></span>

<span data-ttu-id="c0656-261">Nell'API seguente viene illustrato questo comportamento:</span><span class="sxs-lookup"><span data-stu-id="c0656-261">The following API that illustrates this behavior:</span></span>

```csharp
[HttpGet("loh/{size=85000}")]
public int GetLOH1(int size)
{
   return new byte[size].Length;
}
```

<span data-ttu-id="c0656-262">Il grafico seguente mostra il profilo di memoria per chiamare l'endpoint di `/api/loh/84975`, in carico massimo:</span><span class="sxs-lookup"><span data-stu-id="c0656-262">The following chart shows the memory profile of calling the `/api/loh/84975` endpoint, under maximum load:</span></span>

![grafico precedente](memory/_static/loh1.png)

<span data-ttu-id="c0656-264">Il grafico seguente mostra il profilo di memoria per chiamare l'endpoint `/api/loh/84976`, allocando *solo un byte*:</span><span class="sxs-lookup"><span data-stu-id="c0656-264">The following chart shows the memory profile of calling the `/api/loh/84976` endpoint, allocating *just one more byte*:</span></span>

![grafico precedente](memory/_static/loh2.png)

<span data-ttu-id="c0656-266">Nota: la struttura `byte[]` dispone di byte di overhead.</span><span class="sxs-lookup"><span data-stu-id="c0656-266">Note: The `byte[]` structure has overhead bytes.</span></span> <span data-ttu-id="c0656-267">Per questo motivo 84.976 byte attiva il limite di 85.000.</span><span class="sxs-lookup"><span data-stu-id="c0656-267">That's why 84,976 bytes triggers the 85,000 limit.</span></span>

<span data-ttu-id="c0656-268">Confronto tra i due grafici precedenti:</span><span class="sxs-lookup"><span data-stu-id="c0656-268">Comparing the two preceding charts:</span></span>

* <span data-ttu-id="c0656-269">Il working set è simile per entrambi gli scenari, circa 450 MB.</span><span class="sxs-lookup"><span data-stu-id="c0656-269">The working set is similar for both scenarios, about 450 MB.</span></span>
* <span data-ttu-id="c0656-270">Il sotto richieste LOH (84.975 byte) Mostra per lo più le raccolte di generazione 0.</span><span class="sxs-lookup"><span data-stu-id="c0656-270">The under LOH requests (84,975 bytes) shows mostly generation 0 collections.</span></span>
* <span data-ttu-id="c0656-271">Le richieste over LOH generano raccolte di generazione 2 costanti.</span><span class="sxs-lookup"><span data-stu-id="c0656-271">The over LOH requests generate constant generation 2 collections.</span></span> <span data-ttu-id="c0656-272">Le raccolte di generazione 2 sono costose.</span><span class="sxs-lookup"><span data-stu-id="c0656-272">Generation 2 collections are expensive.</span></span> <span data-ttu-id="c0656-273">È necessaria una quantità maggiore di CPU e la velocità effettiva scende quasi al 50%.</span><span class="sxs-lookup"><span data-stu-id="c0656-273">More CPU is required and throughput drops almost 50%.</span></span>

<span data-ttu-id="c0656-274">Gli oggetti temporanei di grandi dimensioni sono particolarmente problematici perché comportano l'Gen2.</span><span class="sxs-lookup"><span data-stu-id="c0656-274">Temporary large objects are particularly problematic because they cause gen2 GCs.</span></span>

<span data-ttu-id="c0656-275">Per ottenere prestazioni ottimali, è necessario ridurre al minimo l'utilizzo di oggetti di grandi dimensioni.</span><span class="sxs-lookup"><span data-stu-id="c0656-275">For maximum performance, large object use should be minimized.</span></span> <span data-ttu-id="c0656-276">Se possibile, suddividere oggetti di grandi dimensioni.</span><span class="sxs-lookup"><span data-stu-id="c0656-276">If possible, split up large objects.</span></span> <span data-ttu-id="c0656-277">Il middleware di [memorizzazione](xref:performance/caching/response) nella cache delle risposte, ad esempio, in ASP.NET Core suddivide le voci della cache in blocchi inferiori a 85.000 byte.</span><span class="sxs-lookup"><span data-stu-id="c0656-277">For example, [Response Caching](xref:performance/caching/response) middleware in ASP.NET Core split the cache entries into blocks less than 85,000 bytes.</span></span>

<span data-ttu-id="c0656-278">I collegamenti seguenti illustrano l'approccio ASP.NET Core per mantenere gli oggetti sotto il limite dell'heap oggetti grandi:</span><span class="sxs-lookup"><span data-stu-id="c0656-278">The following links show the ASP.NET Core approach to keeping objects under the LOH limit:</span></span>
- [<span data-ttu-id="c0656-279">ResponseCaching/Streams/StreamUtilities. cs</span><span class="sxs-lookup"><span data-stu-id="c0656-279">ResponseCaching/Streams/StreamUtilities.cs</span></span>](https://github.com/aspnet/AspNetCore/blob/v3.0.0/src/Middleware/ResponseCaching/src/Streams/StreamUtilities.cs#L16)
- [<span data-ttu-id="c0656-280">ResponseCaching/MemoryResponseCache. cs</span><span class="sxs-lookup"><span data-stu-id="c0656-280">ResponseCaching/MemoryResponseCache.cs</span></span>](https://github.com/aspnet/ResponseCaching/blob/c1cb7576a0b86e32aec990c22df29c780af29ca5/src/Microsoft.AspNetCore.ResponseCaching/Internal/MemoryResponseCache.cs#L55)

<span data-ttu-id="c0656-281">Per altre informazioni, vedere:</span><span class="sxs-lookup"><span data-stu-id="c0656-281">For more information, see:</span></span>

* [<span data-ttu-id="c0656-282">Heap Large Object individuato</span><span class="sxs-lookup"><span data-stu-id="c0656-282">Large Object Heap Uncovered</span></span>](https://devblogs.microsoft.com/dotnet/large-object-heap-uncovered-from-an-old-msdn-article/)
* [<span data-ttu-id="c0656-283">Heap oggetti grandi</span><span class="sxs-lookup"><span data-stu-id="c0656-283">Large object heap</span></span>](/dotnet/standard/garbage-collection/large-object-heap)

### <a name="httpclient"></a><span data-ttu-id="c0656-284">HttpClient</span><span class="sxs-lookup"><span data-stu-id="c0656-284">HttpClient</span></span>

<span data-ttu-id="c0656-285">L'uso errato di <xref:System.Net.Http.HttpClient> può causare una perdita di risorse.</span><span class="sxs-lookup"><span data-stu-id="c0656-285">Incorrectly using <xref:System.Net.Http.HttpClient> can result in a resource leak.</span></span> <span data-ttu-id="c0656-286">Risorse di sistema, ad esempio connessioni di database, socket, handle di file e così via:</span><span class="sxs-lookup"><span data-stu-id="c0656-286">System resources, such as database connections, sockets, file handles, etc.:</span></span>

* <span data-ttu-id="c0656-287">Sono più limitate della memoria.</span><span class="sxs-lookup"><span data-stu-id="c0656-287">Are more scarce than memory.</span></span>
* <span data-ttu-id="c0656-288">Sono più problematiche quando si verifica una perdita di memoria.</span><span class="sxs-lookup"><span data-stu-id="c0656-288">Are more problematic when leaked than memory.</span></span>

<span data-ttu-id="c0656-289">Gli sviluppatori .NET esperti sanno di chiamare <xref:System.IDisposable.Dispose*> sugli oggetti che implementano <xref:System.IDisposable>.</span><span class="sxs-lookup"><span data-stu-id="c0656-289">Experienced .NET developers know to call <xref:System.IDisposable.Dispose*> on objects that implement <xref:System.IDisposable>.</span></span> <span data-ttu-id="c0656-290">Se non si eliminano oggetti che implementano `IDisposable` in genere si verifica una perdita di memoria o di risorse di sistema perse.</span><span class="sxs-lookup"><span data-stu-id="c0656-290">Not disposing objects that implement `IDisposable` typically results in leaked memory or leaked system resources.</span></span>

<span data-ttu-id="c0656-291">`HttpClient` implementa `IDisposable`, ma **non** deve essere eliminato a ogni chiamata.</span><span class="sxs-lookup"><span data-stu-id="c0656-291">`HttpClient` implements `IDisposable`, but should **not** be disposed on every invocation.</span></span> <span data-ttu-id="c0656-292">È invece consigliabile riutilizzare `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="c0656-292">Rather, `HttpClient` should be reused.</span></span>

<span data-ttu-id="c0656-293">L'endpoint seguente crea e Elimina una nuova istanza di `HttpClient` per ogni richiesta:</span><span class="sxs-lookup"><span data-stu-id="c0656-293">The following endpoint creates and disposes a new  `HttpClient` instance on every request:</span></span>

```csharp
[HttpGet("httpclient1")]
public async Task<int> GetHttpClient1(string url)
{
    using (var httpClient = new HttpClient())
    {
        var result = await httpClient.GetAsync(url);
        return (int)result.StatusCode;
    }
}
```

<span data-ttu-id="c0656-294">Sotto carico vengono registrati i messaggi di errore seguenti:</span><span class="sxs-lookup"><span data-stu-id="c0656-294">Under load, the following error messages are logged:</span></span>

```
fail: Microsoft.AspNetCore.Server.Kestrel[13]
      Connection id "0HLG70PBE1CR1", Request id "0HLG70PBE1CR1:00000031":
      An unhandled exception was thrown by the application.
System.Net.Http.HttpRequestException: Only one usage of each socket address
    (protocol/network address/port) is normally permitted --->
    System.Net.Sockets.SocketException: Only one usage of each socket address
    (protocol/network address/port) is normally permitted
   at System.Net.Http.ConnectHelper.ConnectAsync(String host, Int32 port,
    CancellationToken cancellationToken)
```

<span data-ttu-id="c0656-295">Anche se le istanze di `HttpClient` vengono eliminate, la connessione di rete effettiva richiede del tempo per essere rilasciata dal sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="c0656-295">Even though the `HttpClient` instances are disposed, the actual network connection takes some time to be released by the operating system.</span></span> <span data-ttu-id="c0656-296">Creando continuamente nuove connessioni, si verifica l' _esaurimento delle porte_ .</span><span class="sxs-lookup"><span data-stu-id="c0656-296">By continuously creating new connections, _ports exhaustion_ occurs.</span></span> <span data-ttu-id="c0656-297">Ogni connessione client richiede una propria porta client.</span><span class="sxs-lookup"><span data-stu-id="c0656-297">Each client connection requires its own client port.</span></span>

<span data-ttu-id="c0656-298">Un modo per evitare l'esaurimento delle porte è riutilizzare la stessa istanza di `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="c0656-298">One way to prevent port exhaustion is to reuse the same `HttpClient` instance:</span></span>

```csharp
private static readonly HttpClient _httpClient = new HttpClient();

[HttpGet("httpclient2")]
public async Task<int> GetHttpClient2(string url)
{
    var result = await _httpClient.GetAsync(url);
    return (int)result.StatusCode;
}
```

<span data-ttu-id="c0656-299">L'istanza di `HttpClient` viene rilasciata quando l'app viene arrestata.</span><span class="sxs-lookup"><span data-stu-id="c0656-299">The `HttpClient` instance is released when the app stops.</span></span> <span data-ttu-id="c0656-300">Questo esempio mostra che non tutte le risorse eliminabili devono essere eliminate dopo ogni uso.</span><span class="sxs-lookup"><span data-stu-id="c0656-300">This example shows that not every disposable resource should be disposed after each use.</span></span>

<span data-ttu-id="c0656-301">Per un modo migliore per gestire la durata di un'istanza di `HttpClient`, vedere quanto segue:</span><span class="sxs-lookup"><span data-stu-id="c0656-301">See the following for a better way to handle the lifetime of an `HttpClient` instance:</span></span>

* [<span data-ttu-id="c0656-302">Gestione HttpClient e durata</span><span class="sxs-lookup"><span data-stu-id="c0656-302">HttpClient and lifetime management</span></span>](/aspnet/core/fundamentals/http-requests#httpclient-and-lifetime-management)
* [<span data-ttu-id="c0656-303">Blog di HTTPClient Factory</span><span class="sxs-lookup"><span data-stu-id="c0656-303">HTTPClient factory blog</span></span>](https://devblogs.microsoft.com/aspnet/asp-net-core-2-1-preview1-introducing-httpclient-factory/)
 
### <a name="object-pooling"></a><span data-ttu-id="c0656-304">Pool di oggetti</span><span class="sxs-lookup"><span data-stu-id="c0656-304">Object pooling</span></span>

<span data-ttu-id="c0656-305">Nell'esempio precedente è stato illustrato come l'istanza di `HttpClient` può essere resa statica e riutilizzata da tutte le richieste.</span><span class="sxs-lookup"><span data-stu-id="c0656-305">The previous example showed how the `HttpClient` instance can be made static and reused by all requests.</span></span> <span data-ttu-id="c0656-306">Il riutilizzo impedisce l'esaurimento delle risorse.</span><span class="sxs-lookup"><span data-stu-id="c0656-306">Reuse prevents running out of resources.</span></span>

<span data-ttu-id="c0656-307">Pool di oggetti:</span><span class="sxs-lookup"><span data-stu-id="c0656-307">Object pooling:</span></span>

* <span data-ttu-id="c0656-308">Usa il modello di riutilizzo.</span><span class="sxs-lookup"><span data-stu-id="c0656-308">Uses the reuse pattern.</span></span>
* <span data-ttu-id="c0656-309">È progettato per gli oggetti che sono costosi da creare.</span><span class="sxs-lookup"><span data-stu-id="c0656-309">Is designed for objects that are expensive to create.</span></span>

<span data-ttu-id="c0656-310">Un pool è una raccolta di oggetti pre-inizializzati che possono essere riservati e rilasciati tra thread.</span><span class="sxs-lookup"><span data-stu-id="c0656-310">A pool is a collection of pre-initialized objects that can be reserved and released across threads.</span></span> <span data-ttu-id="c0656-311">I pool possono definire regole di allocazione quali limiti, dimensioni predefinite o tasso di crescita.</span><span class="sxs-lookup"><span data-stu-id="c0656-311">Pools can define allocation rules such as limits, predefined sizes, or growth rate.</span></span>

<span data-ttu-id="c0656-312">Il pacchetto NuGet [Microsoft. Extensions. Objectpool](https://www.nuget.org/packages/Microsoft.Extensions.ObjectPool/) contiene classi che consentono di gestire tali pool.</span><span class="sxs-lookup"><span data-stu-id="c0656-312">The NuGet package [Microsoft.Extensions.ObjectPool](https://www.nuget.org/packages/Microsoft.Extensions.ObjectPool/) contains classes that help to manage such pools.</span></span>

<span data-ttu-id="c0656-313">L'endpoint API seguente crea un'istanza di un buffer di `byte` riempito con numeri casuali per ogni richiesta:</span><span class="sxs-lookup"><span data-stu-id="c0656-313">The following API endpoint instantiates a `byte` buffer that is filled with random numbers on each request:</span></span>

```csharp
        [HttpGet("array/{size}")]
        public byte[] GetArray(int size)
        {
            var random = new Random();
            var array = new byte[size];
            random.NextBytes(array);

            return array;
        }
```

<span data-ttu-id="c0656-314">Il grafico seguente mostra la chiamata all'API precedente con carico moderato:</span><span class="sxs-lookup"><span data-stu-id="c0656-314">The following chart display calling the preceding API with moderate load:</span></span>

![grafico precedente](memory/_static/array.png)

<span data-ttu-id="c0656-316">Nel grafico precedente, le raccolte di generazione 0 avvengono approssimativamente una volta al secondo.</span><span class="sxs-lookup"><span data-stu-id="c0656-316">In the preceding chart, generation 0 collections happen approximately once per second.</span></span>

<span data-ttu-id="c0656-317">Il codice precedente può essere ottimizzato raggruppando il buffer `byte` usando [ArrayPool\<t >](xref:System.Buffers.ArrayPool`1).</span><span class="sxs-lookup"><span data-stu-id="c0656-317">The preceding code can be optimized by pooling the `byte` buffer by using [ArrayPool\<T>](xref:System.Buffers.ArrayPool`1).</span></span> <span data-ttu-id="c0656-318">Un'istanza statica viene riutilizzata tra le richieste.</span><span class="sxs-lookup"><span data-stu-id="c0656-318">A static instance is reused across requests.</span></span>

<span data-ttu-id="c0656-319">Ciò che è diverso con questo approccio è che un oggetto in pool viene restituito dall'API.</span><span class="sxs-lookup"><span data-stu-id="c0656-319">What's different with this approach is that a pooled object is returned from the API.</span></span> <span data-ttu-id="c0656-320">Ciò significa che:</span><span class="sxs-lookup"><span data-stu-id="c0656-320">That means:</span></span>

* <span data-ttu-id="c0656-321">L'oggetto è fuori dal controllo non appena si ritorna dal metodo.</span><span class="sxs-lookup"><span data-stu-id="c0656-321">The object is out of your control as soon as you return from the method.</span></span>
* <span data-ttu-id="c0656-322">Non è possibile rilasciare l'oggetto.</span><span class="sxs-lookup"><span data-stu-id="c0656-322">You can't release the object.</span></span>

<span data-ttu-id="c0656-323">Per configurare l'eliminazione dell'oggetto:</span><span class="sxs-lookup"><span data-stu-id="c0656-323">To set up disposal of the object:</span></span>

* <span data-ttu-id="c0656-324">Incapsula la matrice in pool in un oggetto eliminabile.</span><span class="sxs-lookup"><span data-stu-id="c0656-324">Encapsulate the pooled array in a disposable object.</span></span>
* <span data-ttu-id="c0656-325">Registrare l'oggetto in pool con [HttpContext. Response. RegisterForDispose](xref:Microsoft.AspNetCore.Http.HttpResponse.RegisterForDispose*).</span><span class="sxs-lookup"><span data-stu-id="c0656-325">Register the pooled object with [HttpContext.Response.RegisterForDispose](xref:Microsoft.AspNetCore.Http.HttpResponse.RegisterForDispose*).</span></span>

<span data-ttu-id="c0656-326">`RegisterForDispose` si occuperà di chiamare `Dispose`sull'oggetto di destinazione in modo che venga rilasciato solo quando la richiesta HTTP è completa.</span><span class="sxs-lookup"><span data-stu-id="c0656-326">`RegisterForDispose` will take care of calling `Dispose`on the target object so that it's only released when the HTTP request is complete.</span></span>

```csharp
private static ArrayPool<byte> _arrayPool = ArrayPool<byte>.Create();

private class PooledArray : IDisposable
{
    public byte[] Array { get; private set; }

    public PooledArray(int size)
    {
        Array = _arrayPool.Rent(size);
    }

    public void Dispose()
    {
        _arrayPool.Return(Array);
    }
}

[HttpGet("pooledarray/{size}")]
public byte[] GetPooledArray(int size)
{
    var pooledArray = new PooledArray(size);

    var random = new Random();
    random.NextBytes(pooledArray.Array);

    HttpContext.Response.RegisterForDispose(pooledArray);

    return pooledArray.Array;
}
```

<span data-ttu-id="c0656-327">Se si applica lo stesso carico della versione non in pool, viene restituito il grafico seguente:</span><span class="sxs-lookup"><span data-stu-id="c0656-327">Applying the same load as the non-pooled version results in the following chart:</span></span>

![grafico precedente](memory/_static/pooledarray.png)

<span data-ttu-id="c0656-329">La differenza principale è costituita dai byte allocati e, di conseguenza, un numero minore di raccolte di generazione 0.</span><span class="sxs-lookup"><span data-stu-id="c0656-329">The main difference is allocated bytes, and as a consequence much fewer generation 0 collections.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c0656-330">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c0656-330">Additional resources</span></span>

* [<span data-ttu-id="c0656-331">Garbage Collection</span><span class="sxs-lookup"><span data-stu-id="c0656-331">Garbage Collection</span></span>](/dotnet/standard/garbage-collection/)
* [<span data-ttu-id="c0656-332">Informazioni sulle diverse modalità GC con il Visualizzatore di concorrenza</span><span class="sxs-lookup"><span data-stu-id="c0656-332">Understanding different GC modes with Concurrency Visualizer</span></span>](https://blogs.msdn.microsoft.com/seteplia/2017/01/05/understanding-different-gc-modes-with-concurrency-visualizer/)
* [<span data-ttu-id="c0656-333">Heap Large Object individuato</span><span class="sxs-lookup"><span data-stu-id="c0656-333">Large Object Heap Uncovered</span></span>](https://devblogs.microsoft.com/dotnet/large-object-heap-uncovered-from-an-old-msdn-article/)
* [<span data-ttu-id="c0656-334">Heap oggetti grandi</span><span class="sxs-lookup"><span data-stu-id="c0656-334">Large object heap</span></span>](/dotnet/standard/garbage-collection/large-object-heap)
