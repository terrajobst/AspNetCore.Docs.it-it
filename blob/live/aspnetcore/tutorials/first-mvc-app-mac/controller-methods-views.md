---
title: Metodi e viste del controller in un'app ASP.NET Core MVC
author: rick-anderson
description: Utilizzo di metodi, viste e DataAnnotations del controller
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/controller-methods-views
ms.openlocfilehash: 1c58a3bbac227e06a36df0b0d3ed6c2455eb46c4
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="controller-methods-and-views-in-an-aspnet-core-mvc-app"></a><span data-ttu-id="1f667-103">Metodi e viste del controller in un'app ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="1f667-103">Controller methods and views in an ASP.NET Core MVC app</span></span>

<span data-ttu-id="1f667-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1f667-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1f667-105">Le operazioni iniziali con l'app per i film sono state efficaci, ma la presentazione non è ottimale.</span><span class="sxs-lookup"><span data-stu-id="1f667-105">We have a good start to the movie app, but the presentation is not ideal.</span></span> <span data-ttu-id="1f667-106">Deve essere corretto il valore dell'ora (12:00:00 AM nell'immagine seguente) e **ReleaseDate** dovrebbe essere composto da due parole.</span><span class="sxs-lookup"><span data-stu-id="1f667-106">We don't want to see the time (12:00:00 AM in the following image) and **ReleaseDate** should be two words.</span></span>

![Vista Index: Release Date è un'unica parola (senza spazi) e la data di rilascio di ogni film indica l'ora nel formato 12 AM](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

<span data-ttu-id="1f667-108">Aprire il file *Models/Movie.cs* e aggiungere le righe evidenziate illustrate di seguito:</span><span class="sxs-lookup"><span data-stu-id="1f667-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

<span data-ttu-id="1f667-109">Compilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="1f667-109">Build and run the app.</span></span>

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="1f667-110">[Precedente - Utilizzo di SQLite](working-with-sql.md)
[Successivo - Aggiungere la funzionalità di ricerca](search.md)</span><span class="sxs-lookup"><span data-stu-id="1f667-110">[Previous - Working with SQLite](working-with-sql.md)
[Next - Add search](search.md)</span></span>
