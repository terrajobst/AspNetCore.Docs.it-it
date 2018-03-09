---
uid: web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
title: "Novità di ASP.NET Web Pages 3.2 | Documenti Microsoft"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/30/2014
ms.topic: article
ms.assetid: a652beff-8e6b-48ad-bfe4-3703f7ccf0a5
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/whats-new-in-aspnet-web-pages-32
msc.type: authoredcontent
ms.openlocfilehash: cdb0e259bbf27d1d3dcf6ada11e6636c9cefcc9c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-pages-32"></a>Novità di ASP.NET Web Pages 3.2
====================
da [Microsoft](https://github.com/microsoft)

In questo argomento vengono descritte le nuove per 3.2 pagine Web ASP.NET, pagine Web 3.2.2 e [pagine Web 3.2.3 beta1](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)

## <a name="aspnet-web-pages-32"></a>ASP.NET Web Pages 3.2

Questa versione consente di correggere un bug e introduce una nuova funzionalità.

## <a name="download"></a>Download

Le funzionalità di runtime vengono rilasciate come pacchetti NuGet in Raccolta NuGet. Seguono tutti i pacchetti di runtime di [controllo delle versioni semantico](http://semver.org/) specifica. Il pacchetto di ASP.NET Web Pages 3.2 presenta la seguente versione: &ldquo;3.2.0&rdquo;. È possibile installare o aggiornare i pacchetti tramite [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebPages/). Questa versione include anche i corrispondenti pacchetti localizzati in NuGet.

È possibile installare o aggiornare i pacchetti NuGet rilasciati tramite la Console di gestione pacchetti NuGet:

[!code-console[Main](whats-new-in-aspnet-web-pages-32/samples/sample1.cmd)]

## <a name="new-feature-and-bug-fix"></a>Nuova funzionalità e la correzione del Bug

È stato corretto un bug e apportate a uno dei miglioramenti delle lieve in questa versione. È possibile trovare la query per lo stesso [qui](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC|v5.2%20RTM&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=Id&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed).

## <a name="aspnet-web-pages-322"></a>ASP.NET Web Pages 3.2.2

Questa versione il rollup della modifica [versione Beta di ASP.NET Web Pages 3.2.1](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) che fornisce un miglioramento significativo delle prestazioni per il rendering delle pagine di grandi dimensioni razor. Vedere[Codeplex problema 585](https://aspnetwebstack.codeplex.com/workitem/585). Questa versione consente di allineare con MVC 5.2.2 pacchetti che ora si baserà su questa versione.

Abbiamo lavorato con il team MSN il rendering di pagine di grandi dimensioni. Quando il rendering di pagine oltre 80 kilobyte di dati, si finisce con gli oggetti nell'heap degli oggetti di grandi dimensioni. Quando si utilizzano più livelli di layout questo effetto può essere moltiplicato.

Il risultato nel server è l'utilizzo della CPU aggiuntiva, conservazione più lungo di memoria e anche condizione viene sospesa durante [pulizia di generazione 2](https://msdn.microsoft.com/en-us/library/ms973837.aspx) nel garbage collector.

Di seguito è una tabella che illustra i risultati dell'analisi di un [perfview](https://channel9.msdn.com/Series/PerfView-Tutorial) per un'esecuzione. La CPU viene mantenuta costante circa 68%, mentre vengono eseguito il rendering di pagine di grandi dimensioni. La tabella mostra che il numero di raccolte di generazione 2 è stata completamente eliminato e il risultato è maggiore velocità di richiesta e una riduzione notevole pause a causa di operazioni di garbage collection.

| **Area** | **Prima di (3.2)** | **Dopo aver (3.2.1)** | **% Delta** |
| --- | --- | --- | --- |
| Richiesta totale (conteggio) | 26,986 | 32,591 | <font style="background-color: #4bacc6">20.80%</font> |
| Analisi intervallo (secondi) | 196.20 | 198.60 | 1.20% |
| Richieste al secondo | 137.53 | 164.10 | <font style="background-color: #4bacc6">19.30%</font> |
| Carico della CPU | 68.80% | 68.50% |  -0.40% |
| Esempi di CPU GC | 124,323 | 17,543 | <font style="background-color: #4bacc6">-85.90%</font> |
| Allocazioni totali (conteggio) | 55,357,146 | 57,222,949 | 3.40% |
| Totale Pause di Garbage Collection (esempi) | 15,091 | 8,515 | <font style="background-color: #4bacc6">-43.60%</font> |
| GC Gen0 (conteggio) | 403 | 1,216 | 201.70% |
| GC Gen1 (conteggio) | 290 | 367 | 26.60% |
| GC Gen2 (conteggio) | 229 | 2 | <font style="background-color: #00ff00">-99.10%</font> |
| CPU / richiesta (esempi/req) | 19.73 | 16.47 | -16.50% |

| Codifica a colori: | <font style="background-color: #00ff00">Utilizzo di componenti di base</font> | <font style="background-color: #4bacc6">Positivo impatto sulle prestazioni</font> |
| --- | --- | --- |

## <a name="aspnet-web-pages-323-beta1"></a>ASP.NET Web Pages 3.2.3 beta1

Questa versione include solo correzioni di bug. È possibile utilizzare [questa query](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20Pages%2FRazor&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) per visualizzare l'elenco dei problemi risolti in questa versione.
