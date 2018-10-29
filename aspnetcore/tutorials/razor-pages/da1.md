---
title: Aggiornare le pagine generate in un'app ASP.NET Core
author: rick-anderson
description: Informazioni su come aggiornare le pagine generate in un'app ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/da1
ms.openlocfilehash: f47a68840a6307b69bc92a7b157037d91dce5422
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477215"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a><span data-ttu-id="37695-103">Aggiornare le pagine generate in un'app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="37695-103">Update the generated pages in an ASP.NET Core app</span></span>

<span data-ttu-id="37695-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="37695-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="37695-105">Le operazioni iniziali con l'app per i film sono state efficaci, ma la presentazione non è ottimale.</span><span class="sxs-lookup"><span data-stu-id="37695-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="37695-106">Deve essere corretto il valore dell'ora (12:00:00 AM nell'immagine seguente) e **ReleaseDate** dovrebbe essere **Release Date** (due parole).</span><span class="sxs-lookup"><span data-stu-id="37695-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![App per i film aperta in Chrome con i dati sui film](sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="37695-108">Aggiornare il codice generato</span><span class="sxs-lookup"><span data-stu-id="37695-108">Update the generated code</span></span>

<span data-ttu-id="37695-109">Aprire il file *Models/Movie.cs* e aggiungere le righe evidenziate illustrate nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="37695-109">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]

::: moniker-end

<span data-ttu-id="37695-110">Fare clic con il pulsante destro del mouse su una riga rossa ondulata > **Azioni rapide e refactoring**.</span><span class="sxs-lookup"><span data-stu-id="37695-110">Right click on a red squiggly line > **Quick Actions and Refactorings**.</span></span>

  ![Menu di scelta rapida con **> Azioni rapide e refactoring**.](da1/qa.png)

<span data-ttu-id="37695-112">Selezionare `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="37695-112">Select `using System.ComponentModel.DataAnnotations;`</span></span>

  ![using System.ComponentModel.DataAnnotations all'inizio dell'elenco](da1/da.png)

  <span data-ttu-id="37695-114">Visual Studio aggiunge `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="37695-114">Visual Studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

[!INCLUDE [model1](~/includes/RP/da2.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="37695-115">[Precedente: Uso di SQL Server LocalDB](xref:tutorials/razor-pages/sql)
> [Successiva: Aggiungere la funzionalità di ricerca](xref:tutorials/razor-pages/search)</span><span class="sxs-lookup"><span data-stu-id="37695-115">[Previous: Working with SQL Server LocalDB](xref:tutorials/razor-pages/sql)
[Next: Add search](xref:tutorials/razor-pages/search)</span></span>
