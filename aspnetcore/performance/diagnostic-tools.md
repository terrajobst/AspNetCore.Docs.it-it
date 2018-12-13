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
# <a name="performance-diagnostic-tools"></a>Strumenti di diagnostica delle prestazioni

Da [Mike Rousos](https://github.com/mjrousos)

Questo articolo elenca gli strumenti per la diagnosi dei problemi di prestazioni in ASP.NET Core.

## <a name="visual-studio-diagnostic-tools"></a>Strumenti di diagnostica di Visual Studio

Il [strumenti di profilatura e diagnostica](/visualstudio/profiling) incorporato in Visual Studio sono un buon punto di partenza analisi dei problemi di prestazioni. Questi strumenti sono potenti e facilmente utilizzabili dall'ambiente di sviluppo Visual Studio. Gli strumenti consente l'analisi dell'utilizzo della CPU, utilizzo della memoria e gli eventi di prestazioni nelle App ASP.NET Core.

Altre informazioni sono disponibili nel [documentazione di Visual Studio](/visualstudio/profiling/profiling-overview).

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/app-insights-overview) fornisce i dati sulle prestazioni approfonditi per l'app. Application Insights raccoglie automaticamente dati su velocità di risposta, frequenze di errori, tempi di risposta delle dipendenze e altro ancora. Application Insights supporta la registrazione di eventi personalizzati e metriche specifiche per l'app.

Application Insights può essere usato in un ambienti diversi:

* Ottimizzato per funzionare in Azure.
* Funziona in ambiente di produzione, sviluppo e gestione temporanea.
* Funziona in locale dal [Visual Studio](/azure/application-insights/app-insights-visual-studio) o in altri ambienti di hosting.

Per altre informazioni, vedere [Application Insights per ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="perfview"></a>PerfView

[PerfView](https://github.com/Microsoft/perfview) è uno strumento di analisi delle prestazioni creato dal team di .NET in modo specifico per la diagnosi dei problemi di prestazioni di .NET. PerfView consente l'analisi della CPU dell'utilizzo, memoria e Garbage Collection comportamento, gli eventi prestazioni e tempo di clock.

Altre informazioni su PerfView e su come iniziare a usare [esercitazioni video relative a PerfView](http://channel9.msdn.com/Series/PerfView-Tutorial) o, vedere il manuale dell'utente disponibile nello strumento oppure [su GitHub](https://github.com/Microsoft/perfview).

## <a name="perfcollect"></a>PerfCollect

PerfView è uno strumento di analisi delle prestazioni utili per gli scenari di .NET, eseguito solo su Windows in modo che non può essere utilizzato per raccogliere le tracce dalle App ASP.NET Core in esecuzione negli ambienti Linux.

[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md) è uno script bash che usa Linux nativo gli strumenti di profilatura ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page) e [LTTng](https://lttng.org/)) per raccogliere tracce su Linux che possa essere analizzato dalla PerfView. PerfCollect è utile quando i problemi di prestazioni visualizzati negli ambienti Linux in cui PerfView non può essere utilizzata direttamente. Al contrario, PerfCollect può raccogliere tracce dalle app .NET Core che vengono quindi analizzate in un computer Windows tramite PerfView.

Sono disponibili altre informazioni su come installare e iniziare a usare PerfCollect [su GitHub](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md).
