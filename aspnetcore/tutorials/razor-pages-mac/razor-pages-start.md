---
title: Introduzione a pagine Razor in ASP.NET Core con macOS con Visual Studio per Mac
author: rick-anderson
description: Informazioni introduttive su pagine Razor in ASP.NET Core con Visual Studio per Mac.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 07/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: 2da9b34f797758c02132d5cf6cc2f2fb2fe6f05a
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/18/2018
---
# <a name="get-started-with-razor-pages-in-aspnet-core-on-macos-with-visual-studio-for-mac"></a>Introduzione a pagine Razor in ASP.NET Core con macOS con Visual Studio per Mac

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Questa esercitazione illustra le nozioni di base della creazione di un'app Web pagine Razor di ASP.NET Core. Si consiglia di leggere [Introduzione all'uso di pagine Razor](xref:mvc/razor-pages/index) prima di iniziare questa esercitazione. Pagine Razor è il modo consigliato per creare l'interfaccia utente per applicazioni Web in ASP.NET Core.

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-razor-web-app"></a>Creare un'app Web Razor

Da un terminale eseguire i comandi seguenti:

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

I comandi precedenti usano [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) per creare ed eseguire un progetto di pagine Razor. Aprire un browser all'indirizzo http://localhost:5000 per visualizzare l'applicazione.

![Pagina Home o di indice](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Aprire il progetto

Premere Ctrl + C per arrestare l'applicazione.

Da Visual Studio, selezionare **File > Apri**, quindi selezionare il file *RazorPagesMovie.csproj*.

### <a name="launch-the-app"></a>Avviare l'app

In Visual Studio selezionare **Esegui > Avvia senza eseguire debug** per avviare l'app. Visual Studio avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5000`.

Nella prossima esercitazione viene aggiunto un modello al progetto.

> [!div class="step-by-step"]
> [Avanti: Aggiunta di un modello](xref:tutorials/razor-pages-mac/model)
