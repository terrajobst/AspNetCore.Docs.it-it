---
uid: webhooks/diagnostics/debugging
title: Webhook ASP.NET debug | Microsoft Docs
author: rick-anderson
description: Come eseguire il debug ASP.NET Webhook.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.openlocfilehash: 517d282fc22703b5861b748aea51023fa0a12a26
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838067"
---
# <a name="aspnet-webhooks-debugging"></a>Webhook ASP.NET debug  

## <a name="debugging-in-azure"></a>Debug in Azure

Per eseguire il debug dell'applicazione Web durante l'esecuzione in Azure, vedere l'esercitazione [risolvere i problemi di un'app web nel servizio App di Azure con Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="debugging-with-source-and-symbols"></a>Debug con simboli e origine

Oltre a debug il proprio codice, è possibile eseguire il debug direttamente in Microsoft ASP.NET WebHooks e in effetti tutti di .NET. Questa opzione funziona indipendentemente dal fatto che si esegue il debug in locale o remoto. Prima di tutto configurare Visual Studio per trovare l'origine e i simboli selezionando **Debug** e quindi **opzioni e impostazioni**. Impostare le opzioni simile al seguente:

![Opzioni e impostazioni](_static/SourceSymbols.png)

Quindi aggiungere un collegamento a [symbolsource.org](http://symbolsource.org) per aver scaricato l'origine e i simboli. Andare alla **simboli** scheda del precedente menu e aggiungere quanto segue come un percorso simboli:

```
http://srv.symbolsource.org/pdb/Public
```

Inoltre, assicurarsi che la directory della cache ha un nome breve. in caso contrario, i nomi dei file possono diventare troppo lunghi che produrrà i simboli non viene caricato. È un percorso di esempio:

```
C:\SymCache
```

Le impostazioni dovrebbero essere simile al seguente:

![Esempio di percorso di File di simboli opzioni](_static/SymSource.png)
