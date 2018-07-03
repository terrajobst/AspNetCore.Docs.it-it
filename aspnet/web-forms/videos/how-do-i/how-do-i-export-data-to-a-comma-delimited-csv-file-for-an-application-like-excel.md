---
uid: web-forms/videos/how-do-i/how-do-i-export-data-to-a-comma-delimited-csv-file-for-an-application-like-excel
title: "[Procedura:] Esportare i dati in un File delimitato da virgole (CSV) per un'applicazione come Excel | Microsoft Docs"
author: rick-anderson
description: In questo video Chris Pels illustra come richiedere i dati da un database o un'altra origine ed esportarlo in un file delimitato da virgole che può essere usato in un'applicazione li...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/22/2009
ms.topic: article
ms.assetid: c9df86ad-aec2-43d5-bb8a-413ebb666673
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-export-data-to-a-comma-delimited-csv-file-for-an-application-like-excel
msc.type: video
ms.openlocfilehash: 7c1f94118c64fee7f4198cd096ae2ef200981ba8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402603"
---
<a name="how-do-i-export-data-to-a-comma-delimited-csv-file-for-an-application-like-excel"></a><span data-ttu-id="31507-103">[Procedura:] Esportare i dati in un File delimitato da virgole (CSV) per un'applicazione come Excel</span><span class="sxs-lookup"><span data-stu-id="31507-103">[How Do I:] Export Data to a Comma Delimited (CSV) File for an Application Like Excel</span></span>
====================
<span data-ttu-id="31507-104">da [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="31507-104">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="31507-105">In questo video Chris Pels illustra come richiedere i dati da un database o un'altra origine ed esportarlo in un file delimitato da virgole che può essere utilizzato in un'applicazione come Excel.</span><span class="sxs-lookup"><span data-stu-id="31507-105">In this video Chris Pels shows how to take data from a database or other source and export it to a comma delimited file that can be used in an application like Excel.</span></span> <span data-ttu-id="31507-106">In primo luogo, viene creato un set di dati come un oggetto DataTable.</span><span class="sxs-lookup"><span data-stu-id="31507-106">First, a set of data is created as a DataTable object.</span></span> <span data-ttu-id="31507-107">Successivamente, viene cancellata la risposta per la richiesta di pagina web corrente e l'intestazione e il tipo di contenuto sono configurate per essere un file csv.</span><span class="sxs-lookup"><span data-stu-id="31507-107">Next, the Response for the current web page request is cleared and the header and content type are configured to be a csv file.</span></span> <span data-ttu-id="31507-108">I dati effettivi viene quindi aggiunto al flusso di risposta scrivendo prima le intestazioni di colonna per il file csv seguiti dai valori dei dati.</span><span class="sxs-lookup"><span data-stu-id="31507-108">Then the actual data is added to the response stream by first writing the column headers for the csv file followed by the data values.</span></span> <span data-ttu-id="31507-109">Questo approccio può essere utile quando gli utenti richiedono un'esportazione dei dati in modo che può essere modificato in locale in un programma, ad esempio Excel.</span><span class="sxs-lookup"><span data-stu-id="31507-109">This approach can be useful when users require an export of data so it can be manipulated locally in a program like Excel.</span></span>

[<span data-ttu-id="31507-110">&#9654;Guarda il video (19 minuti)</span><span class="sxs-lookup"><span data-stu-id="31507-110">&#9654; Watch video (19 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-export-data-to-a-comma-delimited-csv-file-for-an-application-like-excel)
