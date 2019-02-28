---
title: Introduzione ad ASP.NET Core
author: rick-anderson
description: Un'esercitazione rapida per creare ed eseguire una semplice app Hello World usando ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/15/2019
uid: getting-started
---
# <a name="tutorial-get-started-with-aspnet-core"></a>Esercitazione: Introduzione ad ASP.NET Core

Questa esercitazione illustra come usare l'interfaccia della riga di comando di .NET Core per creare un'app Web ASP.NET Core.

Si imparerà a eseguire le operazioni seguenti:

> [!div class="checklist"]
> * Creare un progetto di app Web.
> * Abilitare HTTPS locale.
> * Eseguire l'app.
> * Modificare una pagina Razor.

Al termine, si avrà un'app Web funzionante che viene eseguita nel computer locale.

![Home page dell'app Web](_static/home-page.png)

## <a name="prerequisites"></a>Prerequisiti

* [.NET Core 2.2 SDK](https://www.microsoft.com/net/download/all)

## <a name="create-a-web-app-project"></a>Creare un progetto di app Web

Aprire una shell dei comandi e immettere il comando seguente:

```console
dotnet new webapp -o aspnetcoreapp
```

## <a name="enable-local-https"></a>Abilitare HTTPS locale

Considerare attendibile il certificato di sviluppo HTTPS:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```console
dotnet dev-certs https --trust
```

Il comando precedente consente di visualizzare la finestra di dialogo seguente:

![Finestra di dialogo Avviso di sicurezza](~/getting-started/_static/cert.png)

Selezionare **Sì** se si accetta di considerare attendibile il certificato di sviluppo.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```console
dotnet dev-certs https --trust
```

Il comando precedente consente di visualizzare il messaggio seguente:

*Trusting the HTTPS development certificate was requested (È stato richiesto di considerare attendibile il certificato di sviluppo HTTPS). Se il certificato non è già stato considerato attendibile, il comando seguente non viene eseguito:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.
 
Questo comando potrebbe richiedere la password per installare il certificato nella keychain di sistema. Inserire la password se si accetta di considerare attendibile il certificato di sviluppo.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Vedere la documentazione della distribuzione di Linux su come rendere attendibile il certificato di sviluppo HTTPS.

---

Per altre informazioni, vedere [Considerare attendibile il certificato di sviluppo di ASP.NET Core HTTPS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)

## <a name="run-the-app"></a>Eseguire l'app

Eseguire i comandi seguenti:

```console
cd aspnetcoreapp
dotnet run
```

Dopo che la shell dei comandi indica che l'app è stata avviata, passare a [https://localhost:5001](https://localhost:5001). Fare clic su **Accept** (Accetto) per accettare l'informativa sulla privacy e la policy per i cookie. Questa app non memorizza informazioni personali.

## <a name="edit-a-razor-page"></a>Modificare una pagina Razor

Aprire *Pages/Index.cshtml* e modificare la pagina con il markup evidenziato seguente:

[!code-cshtml[](sample/index.cshtml?highlight=9)]

Passare a [https://localhost:5001](https://localhost:5001) e verificare che le modifiche siano visualizzate.

## <a name="next-steps"></a>Passaggi successivi

In questa esercitazione si è appreso come:

> [!div class="checklist"]
> * Creare un progetto di app Web.
> * Abilitare HTTPS locale.
> * Eseguire il progetto.
> * Apportare una modifica.

Per altre informazioni su ASP.NET Core, vedere il percorso di apprendimento consigliato nell'introduzione:

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
