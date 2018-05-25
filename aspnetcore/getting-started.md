---
title: Introduzione ad ASP.NET Core
author: rick-anderson
description: Un'esercitazione rapida per creare ed eseguire una semplice app Hello World usando ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: e814277663ff5a964171a71ebb6e0f094e0ddc60
ms.sourcegitcommit: 3d071fabaf90e32906df97b08a8d00e602db25c0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/10/2018
---
# <a name="get-started-with-aspnet-core"></a>Introduzione ad ASP.NET Core

::: moniker range=">= aspnetcore-2.0"

1. Installare [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].

2. Creare un nuovo progetto .NET Core.

   In macOS e Linux aprire una finestra del terminale. In Windows aprire un prompt dei comandi. Immettere il comando seguente:

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```

3. Eseguire l'app con i comandi seguenti:

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. Passare a [http://localhost:5000](http://localhost:5000).

5. Aprire *Pages/About.cshtml* e modificare la pagina in modo che visualizzi il messaggio "Hello, world! The time on the server is@DateTime.Now" (Buongiorno mondo! L'ora nel server è):

    [!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. Passare a [http://localhost:5000/About](http://localhost:5000/About) e verificare le modifiche.

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. Installare il **programma di installazione di SDK** per SDK 1.0.4 dalla [pagina di tutti i download di .NET Core](https://www.microsoft.com/net/download/all).

2. Creare una cartella per un nuovo progetto .NET Core.

   In macOS e Linux aprire una finestra del terminale. In Windows aprire un prompt dei comandi.

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. Se è installata una versione SDK successiva nel computer in uso, creare un file *global.json* per selezionare l'SDK 1.0.4.

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. Creare un nuovo progetto .NET Core.

   ```terminal
   dotnet new web
   ```

5. Ripristinare i pacchetti.

    ```terminal
    dotnet restore
    ```

6. Eseguire l'app.

   ```terminal
   dotnet run
   ```

   Il comando [dotnet run](/dotnet/core/tools/dotnet-run) compila prima l'app se necessario.

7. Passare a `http://localhost:5000`.

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end