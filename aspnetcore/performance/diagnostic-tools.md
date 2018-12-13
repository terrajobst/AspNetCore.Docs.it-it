---
title: Strumenti di diagnostica delle prestazioni
author: mjrousos
description: Strumenti utili per la diagnosi dei problemi di prestazioni nelle App ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 12/07/2018
uid: performance/diagnostic-tools
ms.openlocfilehash: 3093b7d646e4fa943334c7b1e70ddc007ab18780
ms.sourcegitcommit: 1ea1b4fc58055c62728143388562689f1ef96cb2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/13/2018
ms.locfileid: "53329200"
---
# <a name="performance-diagnostic-tools"></a><span data-ttu-id="4a874-103">Strumenti di diagnostica delle prestazioni</span><span class="sxs-lookup"><span data-stu-id="4a874-103">Performance Diagnostic Tools</span></span>

<span data-ttu-id="4a874-104">Da [Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="4a874-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="4a874-105">Questo articolo elenca gli strumenti per la diagnosi dei problemi di prestazioni in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4a874-105">This article lists tools for diagnosing performance issues in ASP.NET Core.</span></span>

## <a name="visual-studio-diagnostic-tools"></a><span data-ttu-id="4a874-106">Strumenti di diagnostica di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4a874-106">Visual Studio Diagnostic Tools</span></span>

<span data-ttu-id="4a874-107">Il [strumenti di profilatura e diagnostica](/visualstudio/profiling) incorporato in Visual Studio sono un buon punto di partenza analisi dei problemi di prestazioni.</span><span class="sxs-lookup"><span data-stu-id="4a874-107">The [profiling and diagnostic tools](/visualstudio/profiling) built into Visual Studio are a good place to start investigating performance issues.</span></span> <span data-ttu-id="4a874-108">Questi strumenti sono potenti e facilmente utilizzabili dall'ambiente di sviluppo Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4a874-108">These tools are powerful and convenient to use from the Visual Studio development environment.</span></span> <span data-ttu-id="4a874-109">Gli strumenti consente l'analisi dell'utilizzo della CPU, utilizzo della memoria e gli eventi di prestazioni nelle App ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4a874-109">The tooling allows analysis of CPU usage, memory usage, and performance events in ASP.NET Core apps.</span></span>

<span data-ttu-id="4a874-110">Altre informazioni sono disponibili nel [documentazione di Visual Studio](/visualstudio/profiling/profiling-overview).</span><span class="sxs-lookup"><span data-stu-id="4a874-110">More information is available in [Visual Studio documentation](/visualstudio/profiling/profiling-overview).</span></span>

## <a name="application-insights"></a><span data-ttu-id="4a874-111">Application Insights</span><span class="sxs-lookup"><span data-stu-id="4a874-111">Application Insights</span></span>

<span data-ttu-id="4a874-112">[Application Insights](/azure/application-insights/app-insights-overview) fornisce i dati sulle prestazioni approfonditi per l'app.</span><span class="sxs-lookup"><span data-stu-id="4a874-112">[Application Insights](/azure/application-insights/app-insights-overview) provides in-depth performance data for your app.</span></span> <span data-ttu-id="4a874-113">Application Insights raccoglie automaticamente dati su velocità di risposta, frequenze di errori, tempi di risposta delle dipendenze e altro ancora.</span><span class="sxs-lookup"><span data-stu-id="4a874-113">Application Insights automatically collects data on response rates, failure rates, dependency response times, and more.</span></span> <span data-ttu-id="4a874-114">Application Insights supporta la registrazione di eventi personalizzati e metriche specifiche per l'app.</span><span class="sxs-lookup"><span data-stu-id="4a874-114">Application Insights supports logging custom events and metrics specific to your app.</span></span>

<span data-ttu-id="4a874-115">Application Insights può essere usato in un ambienti diversi:</span><span class="sxs-lookup"><span data-stu-id="4a874-115">Application Insights can be used in a variety environments:</span></span>

* <span data-ttu-id="4a874-116">Ottimizzato per funzionare in Azure.</span><span class="sxs-lookup"><span data-stu-id="4a874-116">Optimized to work in Azure.</span></span>
* <span data-ttu-id="4a874-117">Funziona in ambiente di produzione, sviluppo e gestione temporanea.</span><span class="sxs-lookup"><span data-stu-id="4a874-117">Works in production, development, and staging.</span></span>
* <span data-ttu-id="4a874-118">Funziona in locale dal [Visual Studio](/azure/application-insights/app-insights-visual-studio) o in altri ambienti di hosting.</span><span class="sxs-lookup"><span data-stu-id="4a874-118">Works locally from [Visual Studio](/azure/application-insights/app-insights-visual-studio) or in other hosting environments.</span></span>

<span data-ttu-id="4a874-119">Per altre informazioni, vedere [Application Insights per ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span><span class="sxs-lookup"><span data-stu-id="4a874-119">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="perfview"></a><span data-ttu-id="4a874-120">PerfView</span><span class="sxs-lookup"><span data-stu-id="4a874-120">PerfView</span></span>

<span data-ttu-id="4a874-121">[PerfView](https://github.com/Microsoft/perfview) è uno strumento di analisi delle prestazioni creato dal team di .NET in modo specifico per la diagnosi dei problemi di prestazioni di .NET.</span><span class="sxs-lookup"><span data-stu-id="4a874-121">[PerfView](https://github.com/Microsoft/perfview) is a performance analysis tool created by the .NET team specifically for diagnosing .NET performance issues.</span></span> <span data-ttu-id="4a874-122">PerfView consente l'analisi della CPU dell'utilizzo, memoria e Garbage Collection comportamento, gli eventi prestazioni e tempo di clock.</span><span class="sxs-lookup"><span data-stu-id="4a874-122">PerfView allows analysis of CPU usage, memory and GC behavior, performance events, and wall clock time.</span></span>

<span data-ttu-id="4a874-123">Altre informazioni su PerfView e su come iniziare a usare [esercitazioni video relative a PerfView](http://channel9.msdn.com/Series/PerfView-Tutorial) o, vedere il manuale dell'utente disponibile nello strumento oppure [su GitHub](https://github.com/Microsoft/perfview).</span><span class="sxs-lookup"><span data-stu-id="4a874-123">You can learn more about PerfView and how to get started with [PerfView video tutorials](http://channel9.msdn.com/Series/PerfView-Tutorial) or by reading the user's guide available in the tool or [on GitHub](https://github.com/Microsoft/perfview).</span></span>

## <a name="perfcollect"></a><span data-ttu-id="4a874-124">PerfCollect</span><span class="sxs-lookup"><span data-stu-id="4a874-124">PerfCollect</span></span>

<span data-ttu-id="4a874-125">PerfView è uno strumento di analisi delle prestazioni utili per gli scenari di .NET, eseguito solo su Windows in modo che non può essere utilizzato per raccogliere le tracce dalle App ASP.NET Core in esecuzione negli ambienti Linux.</span><span class="sxs-lookup"><span data-stu-id="4a874-125">While PerfView is a useful performance analysis tool for .NET scenarios, it only runs on Windows so you can't use it to collect traces from ASP.NET Core apps running in Linux environments.</span></span>

<span data-ttu-id="4a874-126">[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md) è uno script bash che usa Linux nativo gli strumenti di profilatura ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page) e [LTTng](https://lttng.org/)) per raccogliere tracce su Linux che possa essere analizzato dalla PerfView.</span><span class="sxs-lookup"><span data-stu-id="4a874-126">[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md) is a bash script that uses native Linux profiling tools ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page) and [LTTng](https://lttng.org/)) to collect traces on Linux that can be analyzed by PerfView.</span></span> <span data-ttu-id="4a874-127">PerfCollect è utile quando i problemi di prestazioni visualizzati negli ambienti Linux in cui PerfView non può essere utilizzata direttamente.</span><span class="sxs-lookup"><span data-stu-id="4a874-127">PerfCollect is useful when performance problems show up in Linux environments where PerfView can't be used directly.</span></span> <span data-ttu-id="4a874-128">Al contrario, PerfCollect può raccogliere tracce dalle app .NET Core che vengono quindi analizzate in un computer Windows tramite PerfView.</span><span class="sxs-lookup"><span data-stu-id="4a874-128">Instead, PerfCollect can collect traces from .NET Core apps that are then analyzed on a Windows computer using PerfView.</span></span>

<span data-ttu-id="4a874-129">Sono disponibili altre informazioni su come installare e iniziare a usare PerfCollect [su GitHub](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md).</span><span class="sxs-lookup"><span data-stu-id="4a874-129">More information about how to install and get started with PerfCollect is available [on GitHub](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md).</span></span>
