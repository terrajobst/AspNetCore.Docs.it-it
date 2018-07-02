---
title: Introduzione ad ASP.NET Core
author: rick-anderson
description: Un'esercitazione rapida per creare ed eseguire una semplice app Hello World usando ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: eb049dea2800cf2e12c044b88d1664ee80bb95a5
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296768"
---
# <a name="get-started-with-aspnet-core"></a>Introduzione ad ASP.NET Core

::: moniker range=">= aspnetcore-2.1"

1. Installare [!INCLUDE[](~/includes/2.1-SDK.md)].

2. Creare un progetto ASP.NET Core. Aprire una shell dei comandi e immettere il comando seguente:

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

3. Considerare attendibile il certificato di sviluppo HTTPS:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following dialog:

    ![Security warning dialog](_static/cert.png)

    Select **Yes** if you agree to trust the development certificate.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following message:

    *Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:*
    `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`
    *This command might prompt you for your password to install the certificate on the system keychain.
    Password:*

    Enter your password if you agree to trust the development certificate.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

    See the documentation for your Linux distribution on how to trust the HTTPS development certificate
---

4. Eseguire l'app:

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. Passare a [http://localhost:5001](http://localhost:5001).  Fare clic su **Accept** (Accetto) per accettare l'informativa sulla privacy e la policy per i cookie. Questa app non memorizza informazioni personali.

6. Aprire *Pages/About.cshtml* e modificare la pagina con il markup evidenziato seguente:

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. Passare a [http://localhost:5001/About](http://localhost:5001/About) e verificare che le modifiche siano visualizzate.

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. Installare [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].

2. Creare un nuovo progetto ASP.NET Core.

   Aprire una shell dei comandi. Immettere il comando seguente:

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. Eseguire l'app con i comandi seguenti:

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. Passare a [http://localhost:5000](http://localhost:5000).

5. Aprire *Pages/About.cshtml* e modificare la pagina in modo che visualizzi il messaggio "Hello, world! The time on the server is@DateTime.Now" (Buongiorno mondo! L'ora nel server è):

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. Passare a [http://localhost:5000/About](http://localhost:5000/About) e verificare le modifiche.

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. Installare il **programma di installazione di SDK** per SDK 1.0.4 dalla [pagina di tutti i download di .NET Core](https://www.microsoft.com/net/download/all).

2. Creare una cartella per un nuovo progetto ASP.NET Core.

   Aprire una shell dei comandi. Immettere i comandi seguenti:

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. Se è installata una versione SDK successiva nel computer in uso, creare un file *global.json* per selezionare l'SDK 1.0.4.

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. Creare un nuovo progetto ASP.NET Core.

   ```console
   dotnet new web
   ```

5. Ripristinare i pacchetti.

    ```console
    dotnet restore
    ```

6. Eseguire l'app.

   ```console
   dotnet run
   ```

   Il comando [dotnet run](/dotnet/core/tools/dotnet-run) compila prima l'app se necessario.

7. Passare a `http://localhost:5000`.

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
