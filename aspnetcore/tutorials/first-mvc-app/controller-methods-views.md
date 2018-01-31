---
title: Metodi e viste del controller
author: rick-anderson
description: Utilizzo di metodi, viste e DataAnnotations del controller
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 200f02f9815966653b3b46918737c60d11f11d5a
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="controller-methods-and-views"></a><span data-ttu-id="1b322-103">Metodi e viste del controller</span><span class="sxs-lookup"><span data-stu-id="1b322-103">Controller methods and views</span></span>

<span data-ttu-id="1b322-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1b322-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1b322-105">Le operazioni iniziali con l'app per i film sono state efficaci, ma la presentazione non è ottimale.</span><span class="sxs-lookup"><span data-stu-id="1b322-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="1b322-106">Deve essere corretto il valore dell'ora (12:00:00 AM nell'immagine seguente) e **ReleaseDate** dovrebbe essere composto da due parole.</span><span class="sxs-lookup"><span data-stu-id="1b322-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Vista Index: Release Date è un'unica parola (senza spazi) e la data di rilascio di ogni film indica l'ora nel formato 12 AM](working-with-sql/_static/m55.png)

<span data-ttu-id="1b322-108">Aprire il file *Models/Movie.cs* e aggiungere le righe evidenziate illustrate di seguito:</span><span class="sxs-lookup"><span data-stu-id="1b322-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

<span data-ttu-id="1b322-109">Fare clic con il pulsante destro del mouse su una riga rossa ondulata **> Azioni rapide e refactoring**.</span><span class="sxs-lookup"><span data-stu-id="1b322-109">Right click on a red squiggly line **> Quick Actions and Refactorings**.</span></span>

  ![Menu di scelta rapida con **> Azioni rapide e refactoring**.](controller-methods-views/_static/qa.png)


<span data-ttu-id="1b322-111">Toccare `using System.ComponentModel.DataAnnotations;`</span><span class="sxs-lookup"><span data-stu-id="1b322-111">Tap `using System.ComponentModel.DataAnnotations;`</span></span>

  ![using System.ComponentModel.DataAnnotations all'inizio dell'elenco](controller-methods-views/_static/da.png)

  <span data-ttu-id="1b322-113">Visual Studio aggiunge `using System.ComponentModel.DataAnnotations;`.</span><span class="sxs-lookup"><span data-stu-id="1b322-113">Visual studio adds `using System.ComponentModel.DataAnnotations;`.</span></span>

<span data-ttu-id="1b322-114">Rimuovere le istruzioni `using` che non sono necessarie.</span><span class="sxs-lookup"><span data-stu-id="1b322-114">Let's remove the `using` statements that are not needed.</span></span> <span data-ttu-id="1b322-115">Per impostazione predefinita vengono visualizzate con un tipo di carattere grigio chiaro.</span><span class="sxs-lookup"><span data-stu-id="1b322-115">They show up by default in a light grey font.</span></span> <span data-ttu-id="1b322-116">Fare clic con il pulsante destro del mouse in qualsiasi punto del file *Movie.cs* **> Rimuovi e ordina using**.</span><span class="sxs-lookup"><span data-stu-id="1b322-116">Right click anywhere in the *Movie.cs* file **> Remove and Sort Usings**.</span></span>

![Rimuovi e ordina using](controller-methods-views/_static/rm.png)

<span data-ttu-id="1b322-118">Il codice aggiornato:</span><span class="sxs-lookup"><span data-stu-id="1b322-118">The updated code:</span></span>

[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="1b322-119">[Precedente](working-with-sql.md)
[Successivo](search.md)</span><span class="sxs-lookup"><span data-stu-id="1b322-119">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
