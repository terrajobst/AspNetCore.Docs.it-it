---
title: Aggiornare le pagine generate in un'app ASP.NET Core
author: rick-anderson
description: Informazioni su come aggiornare le pagine generate in un'app ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 5/30/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/da1
ms.openlocfilehash: e36982f2b14ec65feb6be0c7f9e942c7a3c9e9ca
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34688432"
---
# <a name="update-the-generated-pages-in-an-aspnet-core-app"></a>Aggiornare le pagine generate in un'app ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Le operazioni iniziali con l'app per i film sono state efficaci, ma la presentazione non è ottimale. Deve essere corretto il valore dell'ora (12:00:00 AM nell'immagine seguente) e **ReleaseDate** dovrebbe essere **Release Date** (due parole).

![App per i film aperta in Chrome con i dati sui film](sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Aggiornare il codice generato

Aprire il file *Models/Movie.cs* e aggiungere le righe evidenziate illustrate nel codice seguente:

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieDate.cs?name=snippet_1&highlight=10-11)]
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/MovieDate.cs?name=snippet_1&highlight=10-11,15)]
::: moniker-end

Fare clic con il pulsante destro del mouse su una riga rossa ondulata > **Azioni rapide e refactoring**.

  ![Menu di scelta rapida con **> Azioni rapide e refactoring**.](da1/qa.png)

Selezionare `using System.ComponentModel.DataAnnotations;`.

  ![using System.ComponentModel.DataAnnotations all'inizio dell'elenco](da1/da.png)

  Visual Studio aggiunge `using System.ComponentModel.DataAnnotations;`.

[!INCLUDE [model1](~/includes/RP/da2.md)]

> [!div class="step-by-step"]
> [Precedente: Utilizzo di SQL Server Local DB](xref:tutorials/razor-pages/sql)
> [Aggiungere la funzionalità di ricerca](xref:tutorials/razor-pages/search)
