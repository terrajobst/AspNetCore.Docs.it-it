---
title: Metodi e viste del controller
author: rick-anderson
description: Utilizzo di metodi, viste e DataAnnotations del controller
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.assetid: c7313211-b271-4adf-bab8-8e72603cc0ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 8960853438d3227de3ef7c50936626149d8d5997
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="controller-methods-and-views"></a><span data-ttu-id="98bbd-104">Metodi e viste del controller</span><span class="sxs-lookup"><span data-stu-id="98bbd-104">Controller methods and views</span></span>

<span data-ttu-id="98bbd-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="98bbd-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="98bbd-106">Le operazioni iniziali con l'app per i film sono state efficaci, ma la presentazione non è ottimale.</span><span class="sxs-lookup"><span data-stu-id="98bbd-106">We have a good start to the movie app, but the presentation is not ideal.</span></span> <span data-ttu-id="98bbd-107">Deve essere corretto il valore dell'ora (12:00:00 AM nell'immagine seguente) e **ReleaseDate** dovrebbe essere composto da due parole.</span><span class="sxs-lookup"><span data-stu-id="98bbd-107">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Vista Index: Release Date è un'unica parola (senza spazi) e la data di rilascio di ogni film indica l'ora nel formato 12 AM](working-with-sql/_static/m55.png)

<span data-ttu-id="98bbd-109">Aprire il file *Models/Movie.cs* e aggiungere le righe evidenziate illustrate di seguito:</span><span class="sxs-lookup"><span data-stu-id="98bbd-109">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

<span data-ttu-id="98bbd-110">Fare clic con il pulsante destro del mouse su una riga rossa ondulata **> Azioni rapide e refactoring**.</span><span class="sxs-lookup"><span data-stu-id="98bbd-110">Right click on a red squiggly line **> Quick Actions and Refactorings**.</span></span>

  ![Menu di scelta rapida con **> Azioni rapide e refactoring**.](controller-methods-views/_static/qa.png)


<span data-ttu-id="98bbd-112">Toccare `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="98bbd-112">Tap `using System.ComponentModel.DataAnnotations;`</span></span>

  ![using System.ComponentModel.DataAnnotations all'inizio dell'elenco](controller-methods-views/_static/da.png)

  <span data-ttu-id="98bbd-114">Visual Studio aggiunge `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="98bbd-114">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="98bbd-115">Rimuovere le istruzioni `using` che non sono necessarie.</span><span class="sxs-lookup"><span data-stu-id="98bbd-115">Let's remove the `using` statements that are not needed.</span></span> <span data-ttu-id="98bbd-116">Per impostazione predefinita vengono visualizzate con un tipo di carattere grigio chiaro.</span><span class="sxs-lookup"><span data-stu-id="98bbd-116">They show up by default in a light grey font.</span></span> <span data-ttu-id="98bbd-117">Fare clic con il pulsante destro del mouse in qualsiasi punto del file *Movie.cs* **> Rimuovi e ordina using**.</span><span class="sxs-lookup"><span data-stu-id="98bbd-117">Right click anywhere in the *Movie.cs* file **> Remove and Sort Usings**.</span></span>

![Rimuovi e ordina using](controller-methods-views/_static/rm.png)

<span data-ttu-id="98bbd-119">Il codice aggiornato:</span><span class="sxs-lookup"><span data-stu-id="98bbd-119">The updated code:</span></span>

[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="98bbd-120">[Precedente](working-with-sql.md)
[Successivo](search.md)</span><span class="sxs-lookup"><span data-stu-id="98bbd-120">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
