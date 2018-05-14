---
title: Introduzione ad ASP.NET Core 1.1
author: rick-anderson
description: Seguire questa esercitazione rapida per creare ed eseguire una semplice app Hello World usando ASP.NET Core 1.1.
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started-1.1
ms.openlocfilehash: c61a9a918e51bbd6c1f1142a04473393c8fc54ca
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/22/2018
---
# <a name="get-started-with-aspnet-core-11"></a>Introduzione ad ASP.NET Core 1.1

> [!NOTE]
> Queste istruzioni sono relative ad ASP.NET Core 1.1. Per cercare la versione più recente, vedere la [versione corrente di questa esercitazione](xref:getting-started).

1. Installare il **programma di installazione di SDK** per SDK 1.0.4 dalla [pagina di tutti i download di .NET Core](https://www.microsoft.com/net/download/all).

2. Creare una cartella per un nuovo progetto .NET Core.

   In macOS e Linux aprire una finestra del terminale. In Windows aprire un prompt dei comandi.

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. Se è installata una versione SDK successiva nel computer in uso, creare un file *global.json* per selezionare l'SDK 1.0.4.

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. Creare un nuovo progetto .NET Core.

   ```terminal
   dotnet new web
   ```
   
3.  Ripristinare i pacchetti.

    ```terminal
    dotnet restore
    ```

4. Eseguire l'app.

   Il comando [dotnet run](/dotnet/core/tools/dotnet-run) compila prima l'app se necessario.

   ```terminal
   dotnet run
   ```

5. Passare a `http://localhost:5000`.

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a>Passaggi successivi

Per le esercitazioni, introduttive, vedere [Esercitazioni di ASP.NET Core](tutorials/index.md)

Per un'introduzione ai concetti e all'architettura di ASP.NET Core, vedere [Introduzione ad ASP.NET Core](index.md) e [Nozioni fondamentali di ASP.NET Core](fundamentals/index.md).

Un'app ASP.NET Core può usare la libreria di classi base e il runtime di .NET Framework o .NET Core. Per altre informazioni, vedere [Scelta di .NET Core o .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
