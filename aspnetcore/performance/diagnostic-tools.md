---
title: Strumenti di diagnostica delle prestazioni
author: mjrousos
description: Strumenti utili per la diagnosi dei problemi di prestazioni nelle App ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 04/11/2019
uid: performance/diagnostic-tools
ms.openlocfilehash: 66676b5a2b95b87bfbbd50022e279e35a12b9793
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64894188"
---
# <a name="performance-diagnostic-tools"></a>Strumenti di diagnostica delle prestazioni

Da [Mike Rousos](https://github.com/mjrousos)

Questo articolo elenca gli strumenti per la diagnosi dei problemi di prestazioni in ASP.NET Core.

## <a name="visual-studio-diagnostic-tools"></a>Strumenti di diagnostica di Visual Studio

Il [strumenti di profilatura e diagnostica](/visualstudio/profiling) incorporato in Visual Studio sono un buon punto di partenza analisi dei problemi di prestazioni. Questi strumenti sono potenti e facilmente utilizzabili dall'ambiente di sviluppo Visual Studio. Gli strumenti consente l'analisi dell'utilizzo della CPU, utilizzo della memoria e gli eventi di prestazioni nelle App ASP.NET Core. Incorporato in corso rende facile profilatura in fase di sviluppo.

Altre informazioni sono disponibili nel [documentazione di Visual Studio](/visualstudio/profiling/profiling-overview).

## <a name="application-insights"></a>Application Insights

[Application Insights](/azure/application-insights/app-insights-overview) fornisce i dati sulle prestazioni approfonditi per l'app. Application Insights raccoglie automaticamente dati su velocità di risposta, frequenze di errori, tempi di risposta delle dipendenze e altro ancora. Application Insights supporta la registrazione di eventi personalizzati e metriche specifiche per l'app.

Azure Application Insights offre diversi modi per fornire informazioni sulle App monitorata:

- [Mappa delle applicazioni](/azure/application-insights/app-insights-app-map) – aiuta i colli di bottiglia delle prestazioni spot o hot spot di errore in tutti i componenti delle App distribuite.
- [Esplora metriche di Azure](/azure/azure-monitor/platform/metrics-getting-started) è un componente del portale di Microsoft Azure che consente di tracciare grafici, correlare visivamente tendenze, e analisi dei picchi e flessioni in valori delle metriche.
- [Pannello delle prestazioni nel portale di Application Insights](/azure/application-insights/app-insights-tutorial-performance):

  - Mostra i dettagli sulle prestazioni per diverse operazioni nell'app monitorata.
  - Consente di eseguire il drill-in un'unica operazione da controllare tutte le parti o le dipendenze che contribuiscono a un lungo periodo di tempo.
  - Profiler possono essere richiamate da qui per raccogliere le tracce delle prestazioni su richiesta.

- [Azure Application Insights Profiler](/azure/azure-monitor/app/profiler) consente di regolare e on demand la profilatura delle app .NET.  Mostra portale di Azure acquisito le analisi delle prestazioni con gli stack di chiamate e i percorsi critici. I file di traccia possono essere scaricati anche per un'analisi più approfondita con PerfView.

Application Insights può essere usato in un ambienti diversi:

- Ottimizzato per funzionare in Azure.
- Funziona in ambiente di produzione, sviluppo e gestione temporanea.
- Funziona in locale dal [Visual Studio](/azure/application-insights/app-insights-visual-studio) o in altri ambienti di hosting.

Per altre informazioni, vedere [Application Insights per ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).

## <a name="perfview"></a>PerfView

[PerfView](https://github.com/Microsoft/perfview) è uno strumento di analisi delle prestazioni creato dal team di .NET in modo specifico per la diagnosi dei problemi di prestazioni di .NET. PerfView consente l'analisi della CPU dell'utilizzo, memoria e Garbage Collection comportamento, gli eventi prestazioni e tempo di clock.

Altre informazioni su PerfView e su come iniziare a usare [esercitazioni video relative a PerfView](http://channel9.msdn.com/Series/PerfView-Tutorial) o, vedere il manuale dell'utente disponibile nello strumento oppure [su GitHub](https://github.com/Microsoft/perfview).

## <a name="windows-performance-toolkit"></a>Windows Performance Toolkit

[Windows Performance Toolkit](/windows-hardware/test/wpt/) (WPT) è costituito da due componenti: Windows Performance Recorder (WPR) e Windows Performance Analyzer (WPA). Gli strumenti realizzati i profili approfondite ad alte prestazioni di App e sistemi operativi Windows. WPT ha modi più avanzati di visualizzazione dei dati, ma sono meno potenti del PerfView relativo la raccolta di dati.

## <a name="perfcollect"></a>PerfCollect

PerfView è uno strumento di analisi delle prestazioni utili per gli scenari di .NET, eseguito solo su Windows in modo che non può essere utilizzato per raccogliere le tracce dalle App ASP.NET Core in esecuzione negli ambienti Linux.

[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md) è uno script bash che usa Linux nativo gli strumenti di profilatura ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page) e [LTTng](https://lttng.org/)) per raccogliere tracce su Linux che possa essere analizzato dalla PerfView. PerfCollect è utile quando i problemi di prestazioni visualizzati negli ambienti Linux in cui PerfView non può essere utilizzata direttamente. Al contrario, PerfCollect può raccogliere tracce dalle app .NET Core che vengono quindi analizzate in un computer Windows tramite PerfView.

Sono disponibili altre informazioni su come installare e iniziare a usare PerfCollect [su GitHub](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md).

## <a name="other-third-party-performance-tools"></a>Altri strumenti per le prestazioni di terze parti

Di seguito sono elencati alcuni strumenti per le prestazioni di terze parti che sono utili nelle analisi delle prestazioni delle applicazioni .NET Core.

- [MiniProfiler](https://miniprofiler.com/)
- dotTrace e dotMemory di JetBrains
- VTune da Intel
