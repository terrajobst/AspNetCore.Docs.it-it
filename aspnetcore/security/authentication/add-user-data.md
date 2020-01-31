---
title: Aggiungere, scaricare ed eliminare i dati utente all'identità in un progetto ASP.NET Core
author: rick-anderson
description: Informazioni su come aggiungere dati utente personalizzati all'identità in un progetto ASP.NET Core. Eliminare i dati al GDPR.
ms.author: riande
ms.date: 01/28/2020
ms.custom: mvc, seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: e08c02e2e5d4a429aae10c59e7ae3ea48c975067
ms.sourcegitcommit: c81ef12a1b6e6ac838e5e07042717cf492e6635b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2020
ms.locfileid: "76885549"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a>Aggiungere, scaricare ed eliminare dati utente personalizzati all'identità in un progetto ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Questo articolo illustra come:

* Aggiungere dati utente personalizzati per un'app web ASP.NET Core.
* Contrassegnare il modello di dati utente personalizzato con l'attributo <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> in modo che sia disponibile automaticamente per il download e l'eliminazione. Rendere i dati in grado di essere scaricati ed eliminato aiuta a soddisfare [GDPR](xref:security/gdpr) requisiti.

Esempio di progetto viene creato da un'app web Razor Pages, ma le istruzioni sono simili per un'app web ASP.NET Core MVC.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([procedura per il download](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prerequisiti

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [](~/includes/3.0-SDK.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [](~/includes/2.2-SDK.md)]

::: moniker-end

## <a name="create-a-razor-web-app"></a>Creare un'app Web Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

::: moniker range=">= aspnetcore-3.0"

* Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**. Denominare il progetto **App Web 1** se si desidera corrispondere lo spazio dei nomi le [Scarica esempio](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) codice.
* Selezionare **ASP.NET Core applicazione Web** > **OK**
* Selezionare **ASP.NET Core 3,0** nell'elenco a discesa
* Selezionare l' **applicazione Web** > **OK**
* Compilare ed eseguire il progetto.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**. Denominare il progetto **App Web 1** se si desidera corrispondere lo spazio dei nomi le [Scarica esempio](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) codice.
* Selezionare **ASP.NET Core applicazione Web** > **OK**
* Selezionare **ASP.NET Core 2,2** nell'elenco a discesa
* Selezionare l' **applicazione Web** > **OK**
* Compilare ed eseguire il progetto.

::: moniker-end


# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a>Eseguire l'utilità di scaffolding di identità

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dal **Esplora soluzioni**, fare doppio clic sul progetto > **Add** > **nuovo elemento di scaffolding**.
* Dal riquadro sinistro della finestra di dialogo **Aggiungi impalcatura** selezionare **Identity** > **Aggiungi**.
* Nella finestra di dialogo **Aggiungi identità** sono disponibili le opzioni seguenti:
  * Selezionare il file di layout esistente *~/Pages/Shared/_Layout.cshtml*
  * Selezionare i file seguenti per eseguire l'override:
    * **Account/Register**
    * **Account/gestire/indice**
  * Selezionare il **+** per creare un nuovo pulsante **classe contesto dati**. Accettare il tipo (**WebApp1.Models.WebApp1Context** se il progetto viene denominato **App Web 1**).
  * Selezionare il **+** per creare un nuovo pulsante **classe utente**. Accettare il tipo (**WebApp1User** se il progetto viene denominato **App Web 1**) > **Add**.
* Selezionare **Aggiungi**.

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

Se l'utilità di scaffolding di ASP.NET Core non è stato precedentemente installato, installarlo ora:

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Aggiungere un riferimento al pacchetto [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) al file di progetto (con estensione csproj). Eseguire il comando seguente nella directory del progetto:

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Eseguire il comando seguente per elencare le opzioni di utilità di scaffolding di identità:

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

Nella cartella del progetto, eseguire l'utilità di scaffolding di identità:

```dotnetcli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

Seguire le istruzioni disponibili nel [migrazioni UseAuthentication e layout](xref:security/authentication/scaffold-identity#efm) per eseguire la procedura seguente:

* Creare una migrazione e aggiornare il database.
* Aggiungere `UseAuthentication` a `Startup.Configure`.
* Aggiungere `<partial name="_LoginPartial" />` nel file di layout.
* Eseguire il test dell'app:
  * Registrare un utente
  * Selezionare il nuovo nome utente (accanto al **Logout** collegamento). Potrebbe essere necessario espandere la finestra oppure selezionare l'icona della barra di navigazione per visualizzare il nome utente e altri collegamenti.
  * Selezionare il **i dati personali** scheda.
  * Selezionare il **scaricare** pulsante ed esaminare le *PersonalData.json* file.
  * Test di **eliminare** pulsante che consente di eliminare usato per l'accesso utente.

## <a name="add-custom-user-data-to-the-identity-db"></a>Aggiungere dati utente personalizzati per il database di identità

Aggiornamento di `IdentityUser` con proprietà personalizzate della classe derivata. Se il progetto di App Web 1 denominata, il file è denominato *Areas/Identity/Data/WebApp1User.cs*. Aggiornare il file con il codice seguente:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

Le proprietà con l'attributo [personaldata](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) sono:

* Eliminato quando la *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* pagina Razor chiama `UserManager.Delete`.
* Incluso nei dati scaricati dal *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* pagina Razor.

### <a name="update-the-accountmanageindexcshtml-page"></a>Aggiornare la pagina Account/Manage/Index.cshtml

Aggiorna il `InputModel` nelle *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* con il seguente codice evidenziato:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=24-32,48-49,96-104,106)]

Aggiorna il *Areas/Identity/Pages/Account/Manage/Index.cshtml* con il markup evidenziato seguente:

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=18-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

Aggiorna il *Areas/Identity/Pages/Account/Manage/Index.cshtml* con il markup evidenziato seguente:

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

::: moniker-end

### <a name="update-the-accountregistercshtml-page"></a>Aggiornare la pagina Register

Aggiorna il `InputModel` nelle *Areas/Identity/Pages/Account/Register.cshtml.cs* con il seguente codice evidenziato:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=30-38,70-71)]

Aggiorna il *Areas/Identity/Pages/Account/Register.cshtml* con il markup evidenziato seguente:

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

Aggiorna il *Areas/Identity/Pages/Account/Register.cshtml* con il markup evidenziato seguente:

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end


Compilazione del progetto.

### <a name="add-a-migration-for-the-custom-user-data"></a>Aggiungere una migrazione per i dati utente personalizzati

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

In Visual Studio **Console di gestione pacchetti**:

```powershell
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a>Test creare, visualizzare, scaricare ed eliminare i dati utente personalizzati

Eseguire il test dell'app:

* Registrare un nuovo utente.
* Visualizzare i dati utente personalizzati nel `/Identity/Account/Manage` pagina.
* Scaricare e visualizzare i dati personali degli utenti dal `/Identity/Account/Manage/PersonalData` pagina.
