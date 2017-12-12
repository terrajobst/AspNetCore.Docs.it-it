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
# <a name="aspnet-webhooks-debugging"></a><span data-ttu-id="cf972-103">ASP.NET Webhook debug</span><span class="sxs-lookup"><span data-stu-id="cf972-103">ASP.NET WebHooks debugging</span></span>  

## <a name="debugging-in-azure"></a><span data-ttu-id="cf972-104">Debug in Azure</span><span class="sxs-lookup"><span data-stu-id="cf972-104">Debugging in Azure</span></span>

<span data-ttu-id="cf972-105">Per eseguire il debug dell'applicazione Web durante l'esecuzione in Azure, vedere l'esercitazione [risolvere i problemi relativi a un'app web nel servizio App di Azure con Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="cf972-105">To debug your Web Application while running in Azure, please see the tutorial [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="debugging-with-source-and-symbols"></a><span data-ttu-id="cf972-106">Con l'origine e i simboli di debug</span><span class="sxs-lookup"><span data-stu-id="cf972-106">Debugging with Source and Symbols</span></span>

<span data-ttu-id="cf972-107">Oltre a debug il proprio codice, è possibile eseguire il debug direttamente in Microsoft ASP.NET WebHooks e, in realtà tutti .NET.</span><span class="sxs-lookup"><span data-stu-id="cf972-107">In addition to debugging your own code, it is possible to debug directly into Microsoft ASP.NET WebHooks, and in fact all of .NET.</span></span> <span data-ttu-id="cf972-108">Questo procedimento funziona indipendentemente dal fatto se si esegue il debug in locale o remota.</span><span class="sxs-lookup"><span data-stu-id="cf972-108">This works regardless of whether you debug locally or remotely.</span></span> <span data-ttu-id="cf972-109">Innanzitutto, configurare Visual Studio per trovare l'origine e i simboli passando a **Debug** e quindi **opzioni e impostazioni**.</span><span class="sxs-lookup"><span data-stu-id="cf972-109">First, configure Visual Studio to find the source and symbols by going to **Debug** and then **Options and Settings**.</span></span> <span data-ttu-id="cf972-110">Impostare le opzioni simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="cf972-110">Set the options like this:</span></span>

![Opzioni e impostazioni](_static/SourceSymbols.png)

<span data-ttu-id="cf972-112">Quindi aggiungere un collegamento a [symbolsource.org](http://symbolsource.org) per scaricare l'origine e i simboli.</span><span class="sxs-lookup"><span data-stu-id="cf972-112">Then add a link to [symbolsource.org](http://symbolsource.org) for downloading the source and symbols.</span></span> <span data-ttu-id="cf972-113">Passare al **simboli** scheda del menu precedente e aggiungere un percorso simboli le seguenti:</span><span class="sxs-lookup"><span data-stu-id="cf972-113">Go to the **Symbols** tab of the menu above and add the following as a symbol location:</span></span>

```
http://srv.symbolsource.org/pdb/Public
```

<span data-ttu-id="cf972-114">Inoltre, assicurarsi che la directory della cache è un nome breve. in caso contrario i nomi dei file possono diventare troppo lunghi che fa sì che i simboli non viene caricato.</span><span class="sxs-lookup"><span data-stu-id="cf972-114">In addition, make sure that the cache directory has a short name; otherwise the file names can get too long which will cause the symbols to not load.</span></span> <span data-ttu-id="cf972-115">È un percorso di esempio:</span><span class="sxs-lookup"><span data-stu-id="cf972-115">A sample path is:</span></span>

```
C:\SymCache
```

<span data-ttu-id="cf972-116">Le impostazioni dovrebbero essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="cf972-116">The settings should look similar to this:</span></span>

![Esempio di percorso di File di simboli opzioni](_static/SymSource.png)
