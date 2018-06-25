---
title: Metodi e viste del controller in un'app ASP.NET Core MVC
author: rick-anderson
description: Utilizzo di metodi, viste e DataAnnotations del controller
ms.author: riande
ms.date: 04/07/2017
uid: tutorials/first-mvc-app-mac/controller-methods-views
ms.openlocfilehash: 0bd57597c74918829de38764326e35df08ab1e05
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279077"
---
# <a name="controller-methods-and-views-in-an-aspnet-core-mvc-app"></a><span data-ttu-id="53319-103">Metodi e viste del controller in un'app ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="53319-103">Controller methods and views in an ASP.NET Core MVC app</span></span>

<span data-ttu-id="53319-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="53319-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="53319-105">Le operazioni iniziali con l'app per i film sono state efficaci, ma la presentazione non è ottimale.</span><span class="sxs-lookup"><span data-stu-id="53319-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="53319-106">Deve essere corretto il valore dell'ora (12:00:00 AM nell'immagine seguente) e **ReleaseDate** dovrebbe essere composto da due parole.</span><span class="sxs-lookup"><span data-stu-id="53319-106">We don't want to see the time (12:00:00 AM in the following image) and **ReleaseDate** should be two words.</span></span>

![Vista Index: Release Date è un'unica parola (senza spazi) e la data di rilascio di ogni film indica l'ora nel formato 12 AM](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

<span data-ttu-id="53319-108">Aprire il file *Models/Movie.cs* e aggiungere le righe evidenziate illustrate di seguito:</span><span class="sxs-lookup"><span data-stu-id="53319-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

<span data-ttu-id="53319-109">Compilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="53319-109">Build and run the app.</span></span>

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE [adding-model](../../includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="53319-110">[Precedente - Utilizzo di SQLite](working-with-sql.md)
> [Successivo - Aggiungere la funzionalità di ricerca](search.md)</span><span class="sxs-lookup"><span data-stu-id="53319-110">[Previous - Working with SQLite](working-with-sql.md)
[Next - Add search](search.md)</span></span>
