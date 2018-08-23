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
# <a name="optimize-build-performance-for-solution"></a>Ottimizzare le prestazioni di compilazione per la soluzione
Visual Studio 2017 15.8 e successivamente ha aggiunto una nuova voce di menu sotto **compilazione > compilazione ASP.NET > ottimizzare le prestazioni di compilazione per la soluzione**.

![schermata della nuova voce di menu](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

ASP.NET consente di compilare le relative visualizzazioni in fase di esecuzione, ovvero che il progetto ASP.NET è caratterizzata da una copia del compilatore. Tuttavia, in un computer di sviluppo durante la copia del compilatore non corrisponde la copia di Visual Studio, le prestazioni di compilazione sono interessata all'ordine di secondi da 1 a 3 per la compilazione incrementale. Questa funzionalità verrà aggiornata la copia del progetto del compilatore in modo che corrispondano Visual Studio cui dovrebbe velocizzare le compilazioni incrementali.

Questa opzione è applicabile solo per i Framework ASP.NET progetti, non è applicabile ad ASP.NET Core.
