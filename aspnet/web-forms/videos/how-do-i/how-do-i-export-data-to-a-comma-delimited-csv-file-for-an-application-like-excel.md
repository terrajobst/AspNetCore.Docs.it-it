---
uid: web-forms/videos/how-do-i/how-do-i-export-data-to-a-comma-delimited-csv-file-for-an-application-like-excel
title: "[Procedura:] Esportare i dati in un File delimitato da virgole (CSV) per un'applicazione come Excel | Microsoft Docs"
author: rick-anderson
description: In questo video Chris Pels illustra come richiedere i dati da un database o un'altra origine ed esportarlo in un file delimitato da virgole che può essere usato in un'applicazione li...
ms.author: riande
ms.date: 01/22/2009
ms.assetid: c9df86ad-aec2-43d5-bb8a-413ebb666673
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-export-data-to-a-comma-delimited-csv-file-for-an-application-like-excel
msc.type: video
ms.openlocfilehash: 401df30b8f35355ecca5226883bb16b46bf88507
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832097"
---
<a name="how-do-i-export-data-to-a-comma-delimited-csv-file-for-an-application-like-excel"></a><span data-ttu-id="2b780-103">[Procedura:] Esportare i dati in un File delimitato da virgole (CSV) per un'applicazione come Excel</span><span class="sxs-lookup"><span data-stu-id="2b780-103">[How Do I:] Export Data to a Comma Delimited (CSV) File for an Application Like Excel</span></span>
====================
<span data-ttu-id="2b780-104">da [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="2b780-104">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="2b780-105">In questo video Chris Pels illustra come richiedere i dati da un database o un'altra origine ed esportarlo in un file delimitato da virgole che può essere utilizzato in un'applicazione come Excel.</span><span class="sxs-lookup"><span data-stu-id="2b780-105">In this video Chris Pels shows how to take data from a database or other source and export it to a comma delimited file that can be used in an application like Excel.</span></span> <span data-ttu-id="2b780-106">In primo luogo, viene creato un set di dati come un oggetto DataTable.</span><span class="sxs-lookup"><span data-stu-id="2b780-106">First, a set of data is created as a DataTable object.</span></span> <span data-ttu-id="2b780-107">Successivamente, viene cancellata la risposta per la richiesta di pagina web corrente e l'intestazione e il tipo di contenuto sono configurate per essere un file csv.</span><span class="sxs-lookup"><span data-stu-id="2b780-107">Next, the Response for the current web page request is cleared and the header and content type are configured to be a csv file.</span></span> <span data-ttu-id="2b780-108">I dati effettivi viene quindi aggiunto al flusso di risposta scrivendo prima le intestazioni di colonna per il file csv seguiti dai valori dei dati.</span><span class="sxs-lookup"><span data-stu-id="2b780-108">Then the actual data is added to the response stream by first writing the column headers for the csv file followed by the data values.</span></span> <span data-ttu-id="2b780-109">Questo approccio può essere utile quando gli utenti richiedono un'esportazione dei dati in modo che può essere modificato in locale in un programma, ad esempio Excel.</span><span class="sxs-lookup"><span data-stu-id="2b780-109">This approach can be useful when users require an export of data so it can be manipulated locally in a program like Excel.</span></span>

[<span data-ttu-id="2b780-110">&#9654;Guarda il video (19 minuti)</span><span class="sxs-lookup"><span data-stu-id="2b780-110">&#9654; Watch video (19 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-export-data-to-a-comma-delimited-csv-file-for-an-application-like-excel)
