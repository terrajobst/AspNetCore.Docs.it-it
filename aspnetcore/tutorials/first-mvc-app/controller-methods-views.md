---
title: Metodi e viste del controller
author: rick-anderson
description: Utilizzo di metodi, viste e DataAnnotations del controller
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: cfe1838371226334d368dca13bba37c5b1f6fc39
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="controller-methods-and-views"></a><span data-ttu-id="42402-103">Metodi e viste del controller</span><span class="sxs-lookup"><span data-stu-id="42402-103">Controller methods and views</span></span>

<span data-ttu-id="42402-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="42402-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="42402-105">Le operazioni iniziali con l'app per i film sono state efficaci, ma la presentazione non è ottimale.</span><span class="sxs-lookup"><span data-stu-id="42402-105">We have a good start to the movie app, but the presentation is not ideal.</span></span> <span data-ttu-id="42402-106">Deve essere corretto il valore dell'ora (12:00:00 AM nell'immagine seguente) e **ReleaseDate** dovrebbe essere composto da due parole.</span><span class="sxs-lookup"><span data-stu-id="42402-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Vista Index: Release Date è un'unica parola (senza spazi) e la data di rilascio di ogni film indica l'ora nel formato 12 AM](working-with-sql/_static/m55.png)

<span data-ttu-id="42402-108">Aprire il file *Models/Movie.cs* e aggiungere le righe evidenziate illustrate di seguito:</span><span class="sxs-lookup"><span data-stu-id="42402-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

<span data-ttu-id="42402-109">Fare clic con il pulsante destro del mouse su una riga rossa ondulata **> Azioni rapide e refactoring**.</span><span class="sxs-lookup"><span data-stu-id="42402-109">Right click on a red squiggly line **> Quick Actions and Refactorings**.</span></span>

  ![Menu di scelta rapida con **> Azioni rapide e refactoring**.](controller-methods-views/_static/qa.png)


<span data-ttu-id="42402-111">Toccare `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="42402-111">Tap `using System.ComponentModel.DataAnnotations;`</span></span>

  ![using System.ComponentModel.DataAnnotations all'inizio dell'elenco](controller-methods-views/_static/da.png)

  <span data-ttu-id="42402-113">Visual Studio aggiunge `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="42402-113">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="42402-114">Rimuovere le istruzioni `using` che non sono necessarie.</span><span class="sxs-lookup"><span data-stu-id="42402-114">Let's remove the `using` statements that are not needed.</span></span> <span data-ttu-id="42402-115">Per impostazione predefinita vengono visualizzate con un tipo di carattere grigio chiaro.</span><span class="sxs-lookup"><span data-stu-id="42402-115">They show up by default in a light grey font.</span></span> <span data-ttu-id="42402-116">Fare clic con il pulsante destro del mouse in qualsiasi punto del file *Movie.cs* **> Rimuovi e ordina using**.</span><span class="sxs-lookup"><span data-stu-id="42402-116">Right click anywhere in the *Movie.cs* file **> Remove and Sort Usings**.</span></span>

![Rimuovi e ordina using](controller-methods-views/_static/rm.png)

<span data-ttu-id="42402-118">Il codice aggiornato:</span><span class="sxs-lookup"><span data-stu-id="42402-118">The updated code:</span></span>

[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="42402-119">[Precedente](working-with-sql.md)
[Successivo](search.md)</span><span class="sxs-lookup"><span data-stu-id="42402-119">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
