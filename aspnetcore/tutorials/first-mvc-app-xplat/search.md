---
title: Aggiunta della funzionalità di ricerca
author: rick-anderson
description: Illustra come aggiungere la funzionalità di ricerca a una semplice app ASP.NET Core MVC
manager: wpickett
ms.author: riande
ms.date: 04/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/search
ms.openlocfilehash: 71e6074035e7c66fed40673d19c241bfcc585c18
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30896494"
---
[!INCLUDE [adding-model](../../includes/mvc-intro/search1.md)]

Nota: SQLlite distingue tra maiuscole e minuscole, pertanto sarà necessario cercare "Fantasma" e non "fantasma".

[!INCLUDE [adding-model](../../includes/mvc-intro/search2.md)]

Modifica il tag `<form>` nella vista Razor *Views\movie\Index.cshtml* per specificare `method="get"`:

```html
<form asp-controller="Movies" asp-action="Index" method="get">
```

[!INCLUDE [adding-model](../../includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> [Precedente - metodi e viste del controller](controller-methods-views.md)
> [Successivo - Aggiungere un campo](new-field.md)  
