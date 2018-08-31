---
uid: visual-studio/overview/2017/optimize-build-perf
title: Ottimizzare le prestazioni di compilazione per la soluzione
author: AngelosP
description: Ottimizzare le prestazioni di compilazione per la soluzione
ms.author: riande
ms.date: 08/29/2018
msc.type: authoredcontent
ms.openlocfilehash: c1a5cf5e59374b4c0dd7150c5dd62fbde42af555
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312141"
---
# <a name="optimize-build-performance-for-solution"></a>Ottimizzare le prestazioni di compilazione per la soluzione

Visual Studio 2017 15.8 o versioni successive includono una voce di menu: **compilare** > **ASP.NET compilazione** > **ottimizzare le prestazioni di compilazione per la soluzione**.

![schermata della nuova voce di menu](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

ASP.NET consente di compilare le relative visualizzazioni in fase di esecuzione, il che significa che un progetto ASP.NET è caratterizzata da una copia del compilatore. Tuttavia in un computer di sviluppo durante la copia del compilatore non corrisponde la copia di Visual Studio, le prestazioni di compilazione sono interessata all'ordine di secondi da 1 a 3 per la compilazione incrementale. Questa funzionalità Aggiorna la copia del progetto del compilatore in modo che corrispondano Visual Studio, che in genere consente di velocizzare le compilazioni incrementali.

**Questo è applicabile a ASP.NET Framework 4.7.1 o versioni successive solo progetti, non è applicabile ad ASP.NET Core.**
