---
title: Introduzione all'uso di pagine Razor in ASP.NET Core su Mac
author: rick-anderson
description: Introduzione all'uso di pagine Razor in ASP.NET Core con Visual Studio per Mac
ms.author: riande
manager: wpickett
ms.date: 07/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: d4d8c7c543273aa38d1599b51d00a348df7182de
ms.sourcegitcommit: 09b342b45e7372ba9ebf17f35eee331e5a08fb26
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/26/2018
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-for-mac"></a>Introduzione all'uso di pagine Razor in ASP.NET Core con Visual Studio per Mac

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Questa esercitazione illustra le nozioni di base della creazione di un'app Web pagine Razor di ASP.NET Core. Si consiglia di leggere [Introduzione all'uso di pagine Razor](xref:mvc/razor-pages/index) prima di iniziare questa esercitazione. Pagine Razor è il modo consigliato per creare l'interfaccia utente per applicazioni Web in ASP.NET Core.

## <a name="prerequisites"></a>Prerequisiti

Installare gli elementi seguenti:

* [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) o versione successiva
* [Visual Studio per Mac](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-razor-web-app"></a>Creare un'app Web Razor

Da un terminale eseguire i comandi seguenti:

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

I comandi precedenti usano [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) per creare ed eseguire un progetto di pagine Razor. Aprire un browser all'indirizzo http://localhost:5000/ per visualizzare l'applicazione.

![Pagina Home o di indice](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Aprire il progetto

Premere Ctrl + C per arrestare l'applicazione.

Da Visual Studio, selezionare **File > Apri**, quindi selezionare il file *RazorPagesMovie.csproj*.

### <a name="launch-the-app"></a>Avviare l'app

In Visual Studio selezionare **Esegui > Avvia senza eseguire debug** per avviare l'app. Visual Studio avvia [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview), avvia un browser e passa a `http://localhost:5000`.

Nella prossima esercitazione viene aggiunto un modello al progetto.

>[!div class="step-by-step"]
[Avanti: Aggiunta di un modello](xref:tutorials/razor-pages-mac/model)
