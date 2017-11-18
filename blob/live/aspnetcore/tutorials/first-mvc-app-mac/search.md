---
title: "Aggiunta della funzionalità di ricerca a un'app ASP.NET Core MVC"
author: rick-anderson
description: "Illustra come aggiungere la funzionalità di ricerca a una semplice app ASP.NET Core MVC"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: d69e5529-ffff-4628-855d-200202096269
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-mac/search
ms.openlocfilehash: 8dab5293ab6a6fe65288bb230e4f39af462ba28b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/search1.md)]

<span data-ttu-id="edf42-104">Nota: SQLlite distingue tra maiuscole e minuscole, pertanto sarà necessario cercare "Fantasma" e non "fantasma".</span><span class="sxs-lookup"><span data-stu-id="edf42-104">Note: SQLlite is case sensitive, so you'll need to search for "Ghost" and not "ghost".</span></span>

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

<span data-ttu-id="edf42-105">Modifica il tag `<form>` nella vista Razor *Views\movie\Index.cshtml* per specificare `method="get"`:</span><span class="sxs-lookup"><span data-stu-id="edf42-105">Change the `<form>` tag in the *Views\movie\Index.cshtml* Razor view to specify `method="get"`:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
<span data-ttu-id="edf42-106">[Precedente - metodi e viste del controller](controller-methods-views.md)
[Successivo - Aggiungere un campo](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="edf42-106">[Previous - Controller methods and views](controller-methods-views.md)
[Next - Add a field](new-field.md)</span></span>
