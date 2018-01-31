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
ms.openlocfilehash: e279b6c2852f1aea49685381ccaa5f7854f7c418
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="controller-methods-and-views"></a>Metodi e viste del controller

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Le operazioni iniziali con l'app per i film sono state efficaci, ma la presentazione non è ottimale. Deve essere corretto il valore dell'ora (12:00:00 AM nell'immagine seguente) e **ReleaseDate** dovrebbe essere composto da due parole.

![Vista Index: Release Date è un'unica parola (senza spazi) e la data di rilascio di ogni film indica l'ora nel formato 12 AM](working-with-sql/_static/m55.png)

Aprire il file *Models/Movie.cs* e aggiungere le righe evidenziate illustrate di seguito:

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

Fare clic con il pulsante destro del mouse su una riga rossa ondulata **> Azioni rapide e refactoring**.

  ![Menu di scelta rapida con **> Azioni rapide e refactoring**.](controller-methods-views/_static/qa.png)


Toccare `using System.ComponentModel.DataAnnotations;`

  ![using System.ComponentModel.DataAnnotations all'inizio dell'elenco](controller-methods-views/_static/da.png)

  Visual Studio aggiunge `using System.ComponentModel.DataAnnotations;`.

Rimuovere le istruzioni `using` che non sono necessarie. Per impostazione predefinita vengono visualizzate con un tipo di carattere grigio chiaro. Fare clic con il pulsante destro del mouse in qualsiasi punto del file *Movie.cs* **> Rimuovi e ordina using**.

![Rimuovi e ordina using](controller-methods-views/_static/rm.png)

Il codice aggiornato:

[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
[Precedente](working-with-sql.md)
[Successivo](search.md)  
