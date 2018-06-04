---
title: Metodi e viste del controller in ASP.NET Core
author: rick-anderson
description: Informazioni su come usare metodi, visualizzazioni e DataAnnotations del controller in ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 3f84d242a41bc482110d87ff342fa5b5d8c870ff
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729853"
---
# <a name="controller-methods-and-views-in-aspnet-core"></a><span data-ttu-id="561f9-103">Metodi e viste del controller in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="561f9-103">Controller methods and views in ASP.NET Core</span></span>

<span data-ttu-id="561f9-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="561f9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="561f9-105">Le operazioni iniziali con l'app per i film sono state efficaci, ma la presentazione non è ottimale.</span><span class="sxs-lookup"><span data-stu-id="561f9-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="561f9-106">Deve essere corretto il valore dell'ora (12:00:00 AM nell'immagine seguente) e **ReleaseDate** dovrebbe essere composto da due parole.</span><span class="sxs-lookup"><span data-stu-id="561f9-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Vista Index: Release Date è un'unica parola (senza spazi) e la data di rilascio di ogni film indica l'ora nel formato 12 AM](working-with-sql/_static/m55.png)

<span data-ttu-id="561f9-108">Aprire il file *Models/Movie.cs* e aggiungere le righe evidenziate illustrate di seguito:</span><span class="sxs-lookup"><span data-stu-id="561f9-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="561f9-109">[!code-csharp[](start-mvc/sample/MvcMovie21/Models/MovieDateFixed.cs?name=snippet_1&highlight=2,3,12-13,17)]</span><span class="sxs-lookup"><span data-stu-id="561f9-109">[!code-csharp[](start-mvc/sample/MvcMovie21/Models/MovieDateFixed.cs?name=snippet_1&highlight=2,3,12-13,17)]</span></span>
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="561f9-110">[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="561f9-110">[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]</span></span>
::: moniker-end

[!INCLUDE [adding-model](~/includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="561f9-111">[Precedente](working-with-sql.md)
> [Successivo](search.md)</span><span class="sxs-lookup"><span data-stu-id="561f9-111">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
