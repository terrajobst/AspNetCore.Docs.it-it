---
title: Introduzione ad ASP.NET Core
author: rick-anderson
description: Un'esercitazione rapida per creare ed eseguire una semplice app Hello World usando ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 7ab9f303d74786c4ac76f002d0f2c66371e78cb8
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228582"
---
# <a name="get-started-with-aspnet-core"></a>Introduzione ad ASP.NET Core

::: moniker range=">= aspnetcore-2.1"

1. Installare [!INCLUDE [](~/includes/2.1-SDK.md)].

2. Creare un progetto ASP.NET Core. Aprire una shell dei comandi e immettere il comando seguente:

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [!INCLUDE [](~/includes/webapp-alias-notice.md)]

3. Considerare attendibile il certificato di sviluppo HTTPS:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

   Il comando precedente consente di visualizzare la finestra di dialogo seguente:

   ![Finestra di dialogo Avviso di sicurezza](_static/cert.png)

   Selezionare **Sì** se si accetta di considerare attendibile il certificato di sviluppo.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

   Il comando precedente consente di visualizzare il messaggio seguente:

   *Trusting the HTTPS development certificate was requested (È stato richiesto di considerare attendibile il certificato di sviluppo HTTPS). Se il certificato non è già attendibile viene eseguito il comando seguente:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'` *Questo comando potrebbe richiedere la password per installare il certificato nel keychain del sistema.    Password:*

   Inserire la password se si accetta di considerare attendibile il certificato di sviluppo.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

   <a name="see-the-documentation-for-your-linux-distribution-on-how-to-trust-the-https-development-certificate"></a>Vedere la documentazione della distribuzione di Linux su come rendere attendibile il certificato di sviluppo HTTPS
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

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. Installare [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].

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

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

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

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
