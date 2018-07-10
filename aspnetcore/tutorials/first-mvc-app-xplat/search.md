---
title: Aggiungere la funzionalità di ricerca a un'app ASP.NET Core MVC
author: rick-anderson
description: Illustra come aggiungere la funzionalità di ricerca a una semplice app ASP.NET Core MVC
ms.author: riande
ms.date: 04/07/2017
uid: tutorials/first-mvc-app-xplat/search
ms.openlocfilehash: cf4fe3806b45008f48bf5f0598057552bdcfae7c
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961438"
---
[!INCLUDE [adding-model](../../includes/mvc-intro/search1.md)]

<span data-ttu-id="323d1-103">Nota: SQLlite distingue tra maiuscole e minuscole, pertanto sarà necessario cercare "Fantasma" e non "fantasma".</span><span class="sxs-lookup"><span data-stu-id="323d1-103">Note: SQLlite is case sensitive, so you'll need to search for "Ghost" and not "ghost".</span></span>

[!INCLUDE [adding-model](../../includes/mvc-intro/search2.md)]

<span data-ttu-id="323d1-104">Modifica il tag `<form>` nella vista Razor *Views\movie\Index.cshtml* per specificare `method="get"`:</span><span class="sxs-lookup"><span data-stu-id="323d1-104">Change the `<form>` tag in the *Views\movie\Index.cshtml* Razor view to specify `method="get"`:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE [adding-model](../../includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="323d1-105">[Precedente - metodi e viste del controller](controller-methods-views.md)
> [Successivo - Aggiungere un campo](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="323d1-105">[Previous - Controller methods and views](controller-methods-views.md)
[Next - Add a field](new-field.md)</span></span>  
