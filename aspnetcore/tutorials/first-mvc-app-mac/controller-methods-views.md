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
ms.openlocfilehash: 71cdf9f0a4a72f375af094c7c0a446278f8aeeb5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="controller-methods-and-views-in-an-aspnet-core-mvc-app"></a>Metodi e viste del controller in un'app ASP.NET Core MVC

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Le operazioni iniziali con l'app per i film sono state efficaci, ma la presentazione non è ottimale. Deve essere corretto il valore dell'ora (12:00:00 AM nell'immagine seguente) e **ReleaseDate** dovrebbe essere composto da due parole.

![Vista Index: Release Date è un'unica parola (senza spazi) e la data di rilascio di ogni film indica l'ora nel formato 12 AM](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

Aprire il file *Models/Movie.cs* e aggiungere le righe evidenziate illustrate di seguito:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

Compilare ed eseguire l'app.

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
[Precedente - Utilizzo di SQLite](working-with-sql.md)
[Successivo - Aggiungere la funzionalità di ricerca](search.md)
