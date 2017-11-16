---
title: Introduzione ad ASP.NET Core 1.1
author: rick-anderson
description: Un'esercitazione rapida per creare ed eseguire una semplice app Hello World usando ASP.NET Core 1.1.
keywords: ASP.NET Core, esercitazione, introduzione
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started-1.1
ms.openlocfilehash: e8fd9ef60ebc1cff6ca0e03000ea50eebff0a9f9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-aspnet-core-11"></a>Introduzione ad ASP.NET Core 1.1

> [!NOTE]
> Queste istruzioni sono relative ad ASP.NET Core 1.1. Per cercare la versione più recente, vedere la [versione corrente di questa esercitazione](xref:getting-started).

1. Installare il **programma di installazione SDK** di .NET Core per SDK 1.0.4 dalla [pagina dei download di .NET Core 1.0.5 e 1.1.2 con SDK 1.0.4](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).

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

   Il comando `dotnet run` compila prima l'app se necessario.

   ```terminal
   dotnet run
   ```

5. Passare a `http://localhost:5000`.

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a>Passaggi successivi

Per le esercitazioni, introduttive, vedere [Esercitazioni di ASP.NET Core](tutorials/index.md)

Per un'introduzione ai concetti e all'architettura di ASP.NET Core, vedere [Introduzione ad ASP.NET Core](index.md) e [Nozioni fondamentali di ASP.NET Core](fundamentals/index.md).

Un'app ASP.NET Core può usare la libreria di classi base e il runtime di .NET Framework o .NET Core. Per altre informazioni, vedere [Scelta di .NET Core o .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
