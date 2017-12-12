---
uid: webhooks/diagnostics/debugging
title: ASP.NET Webhook debug | Documenti Microsoft
author: rick-anderson
description: Come eseguire il debug ASP.NET Webhook.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 566ee353f6a947e3ef0efdfd0af3a81dff2147c7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-debugging"></a>ASP.NET Webhook debug  

## <a name="debugging-in-azure"></a>Debug in Azure

Per eseguire il debug dell'applicazione Web durante l'esecuzione in Azure, vedere l'esercitazione [risolvere i problemi relativi a un'app web nel servizio App di Azure con Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="debugging-with-source-and-symbols"></a>Con l'origine e i simboli di debug

Oltre a debug il proprio codice, è possibile eseguire il debug direttamente in Microsoft ASP.NET WebHooks e, in realtà tutti .NET. Questo procedimento funziona indipendentemente dal fatto se si esegue il debug in locale o remota. Innanzitutto, configurare Visual Studio per trovare l'origine e i simboli passando a **Debug** e quindi **opzioni e impostazioni**. Impostare le opzioni simile al seguente:

![Opzioni e impostazioni](_static/SourceSymbols.png)

Quindi aggiungere un collegamento a [symbolsource.org](http://symbolsource.org) per scaricare l'origine e i simboli. Passare al **simboli** scheda del menu precedente e aggiungere un percorso simboli le seguenti:

```
http://srv.symbolsource.org/pdb/Public
```

Inoltre, assicurarsi che la directory della cache è un nome breve. in caso contrario i nomi dei file possono diventare troppo lunghi che fa sì che i simboli non viene caricato. È un percorso di esempio:

```
C:\SymCache
```

Le impostazioni dovrebbero essere simile al seguente:

![Esempio di percorso di File di simboli opzioni](_static/SymSource.png)
