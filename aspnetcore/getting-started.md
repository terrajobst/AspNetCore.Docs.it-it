---
title: Introduzione ad ASP.NET Core
author: rick-anderson
description: Un'esercitazione rapida per creare ed eseguire una semplice app Hello World usando ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: c2f18c69901a5a6503314d508a776e6985872681
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-aspnet-core"></a>Introduzione ad ASP.NET Core

> [!NOTE]
> Queste istruzioni sono relative alla versione più recente di ASP.NET Core. Vedere [Introduzione ad ASP.NET Core 1.1](xref:getting-started-1.1) per la versione 1.1 di questo documento.

1. Installare [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].

2. Creare un nuovo progetto .NET Core.

   In macOS e Linux aprire una finestra del terminale. In Windows aprire un prompt dei comandi. Immettere il comando seguente:

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
3. Eseguire l'app.

    Usare i comandi seguenti per eseguire l'app:

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. Passare a [http://localhost:5000](http://localhost:5000)

5. Aprire <em>Pages/About.cshtml</em> e modificare la pagina in modo che visualizzi il messaggio "Hello, world! The time on the server is @DateTime.Now " (Buongiorno mondo! L'ora nel server è):

    [!code-html[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. Passare a [http://localhost:5000/About](http://localhost:5000/About) e verificare le modifiche.

### <a name="next-steps"></a>Passaggi successivi

Per le esercitazioni, introduttive, vedere [Esercitazioni di ASP.NET Core](tutorials/index.md)

Per un'introduzione ai concetti e all'architettura di ASP.NET Core, vedere [Introduzione ad ASP.NET Core](index.md) e [Nozioni fondamentali di ASP.NET Core](fundamentals/index.md).

Un'app ASP.NET Core può usare la libreria di classi base e il runtime di .NET Framework o .NET Core. Per altre informazioni, vedere [Scelta di .NET Core o .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
