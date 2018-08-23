---
uid: visual-studio/overview/2017/optimize-build-perf
title: Ottimizzare le prestazioni di compilazione per la soluzione
author: tfitzmac
description: Ottimizzare le prestazioni di compilazione per la soluzione
ms.author: riande
ms.date: 08/22/2018
msc.type: authoredcontent
ms.openlocfilehash: 19f190835e7477e69db470b74edac9e211fd9158
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/22/2018
ms.locfileid: "41909974"
---
# <a name="optimize-build-performance-for-solution"></a><span data-ttu-id="f2269-103">Ottimizzare le prestazioni di compilazione per la soluzione</span><span class="sxs-lookup"><span data-stu-id="f2269-103">Optimize build performance for solution</span></span>
<span data-ttu-id="f2269-104">Visual Studio 2017 15.8 e successivamente ha aggiunto una nuova voce di menu sotto **compilazione > compilazione ASP.NET > ottimizzare le prestazioni di compilazione per la soluzione**.</span><span class="sxs-lookup"><span data-stu-id="f2269-104">Visual Studio 2017 15.8 and later added a new menu item under **Build > ASP.NET Compilation > Optimize Build Performance for Solution**.</span></span>

![schermata della nuova voce di menu](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

<span data-ttu-id="f2269-106">ASP.NET consente di compilare le relative visualizzazioni in fase di esecuzione, ovvero che il progetto ASP.NET è caratterizzata da una copia del compilatore.</span><span class="sxs-lookup"><span data-stu-id="f2269-106">ASP.NET compiles its views at runtime, which means your ASP.NET project carries with it a copy of the compiler.</span></span> <span data-ttu-id="f2269-107">Tuttavia, in un computer di sviluppo durante la copia del compilatore non corrisponde la copia di Visual Studio, le prestazioni di compilazione sono interessata all'ordine di secondi da 1 a 3 per la compilazione incrementale.</span><span class="sxs-lookup"><span data-stu-id="f2269-107">However, on a developer machine when the copy of the compiler doesn't match Visual Studio's copy, your build performance is impacted on the order of 1-3 seconds per incremental build.</span></span> <span data-ttu-id="f2269-108">Questa funzionalità verrà aggiornata la copia del progetto del compilatore in modo che corrispondano Visual Studio cui dovrebbe velocizzare le compilazioni incrementali.</span><span class="sxs-lookup"><span data-stu-id="f2269-108">This feature will update your project's copy of the compiler to match Visual Studio's which should speed up incremental builds.</span></span>

<span data-ttu-id="f2269-109">Questa opzione è applicabile solo per i Framework ASP.NET progetti, non è applicabile ad ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f2269-109">This is applicable to ASP.NET Framework projects only, it does not apply to ASP.NET Core.</span></span>
