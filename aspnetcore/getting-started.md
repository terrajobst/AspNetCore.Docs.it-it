---
title: Introduzione ad ASP.NET Core 2.0
author: rick-anderson
description: Un'esercitazione rapida per creare ed eseguire una semplice app Hello World usando ASP.NET Core.
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: b5f1fb0de2776177374b8b4d5ea6041b0fc653a9
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-aspnet-core"></a>Introduzione ad ASP.NET Core

> [!NOTE]
> Queste istruzioni sono relative alla versione più recente di ASP.NET Core. Per iniziare con una versione precedente, vedere la [versione 1.1 di questa esercitazione](xref:getting-started-1.1).

1. Installare [.NET Core](https://www.microsoft.com/net/core/).

2. Creare un nuovo progetto .NET Core.

   In macOS e Linux aprire una finestra del terminale. In Windows aprire un prompt dei comandi. Immettere il comando seguente:

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
4. Eseguire l'app.

    Usare i comandi seguenti per eseguire l'app:

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

5. Passare a [http://localhost:5000](http://localhost:5000)

6. Aprire *Pages/About.cshtml* e modificare la pagina in modo che visualizzi il messaggio "Hello, world! The time on the server is @DateTime.Now " (Buongiorno mondo! L'ora nel server è):

    [!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

7. Passare a [http://localhost:5000/About](http://localhost:5000/About) e verificare le modifiche.

### <a name="next-steps"></a>Passaggi successivi

Per le esercitazioni, introduttive, vedere [Esercitazioni di ASP.NET Core](tutorials/index.md)

Per un'introduzione ai concetti e all'architettura di ASP.NET Core, vedere [Introduzione ad ASP.NET Core](index.md) e [Nozioni fondamentali di ASP.NET Core](fundamentals/index.md).

Un'app ASP.NET Core può usare la libreria di classi base e il runtime di .NET Framework o .NET Core. Per altre informazioni, vedere [Scelta di .NET Core o .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
