---
title: Aggiungere, scaricare ed eliminare dati dell'utente personalizzati di identità in un progetto ASP.NET Core
author: rick-anderson
description: Informazioni su come aggiungere dati dell'utente personalizzati di identità in un progetto ASP.NET Core. Eliminare i dati per ogni PILR.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 6/16/2018
uid: security/authentication/add-user-data
ms.openlocfilehash: ecd0e6d1c71b24309fab70fbb06af7731463bb0e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36271957"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a>Aggiungere, scaricare ed eliminare dati dell'utente personalizzati di identità in un progetto ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

Questo articolo illustra come:

* Aggiungere dati dell'utente personalizzati per un'app web ASP.NET Core.
* Decorare il modello di dati utente personalizzata con il [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attributo pertanto è automaticamente disponibile per il download e l'eliminazione. Rendere i dati in grado di essere scaricata ed eliminata consente di soddisfare [PILR](xref:security/gdpr) requisiti.

L'esempio di progetto viene creato da un'app web pagine Razor, ma le istruzioni sono simili per un'app web ASP.NET Core MVC.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Prerequisiti

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-razor-web-app"></a>Creare un'app Web Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**. Denominare il progetto **WebApp1** se si desidera corrispondere lo spazio dei nomi il [scaricare esempio](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) codice.
* Selezionare **applicazione Web ASP.NET Core** > **OK**
* Selezionare **ASP.NET Core 2.1** nell'elenco a discesa
* Selezionare **applicazione Web**  > **OK**
* Compilare ed eseguire il progetto.

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

```cli
dotnet new webapp -o WebApp1
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

## <a name="run-the-identity-scaffolder"></a>Eseguire il scaffolder identità

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dal **Esplora soluzioni**, fare clic su progetto > **Add** > **nuovo elemento di scaffolding**.
* Nel riquadro sinistro della **aggiungere lo scaffolding** finestra di dialogo, seleziona **identità** > **aggiungere**.
* Nel **identità Aggiungi** finestra di dialogo, le opzioni seguenti:
  * Selezionare il file di layout esistente *~/Pages/Shared/_Layout.cshtml*
  * Selezionare i file seguenti per eseguire l'override:
    * **Account/Register**
    * **Account/gestire/indice**
  * Selezionare il **+** pulsante per creare un nuovo **classe contesto dati**. Accettare il tipo (**WebApp1.Models.WebApp1Context** se il nome assegnato al progetto **WebApp1**).
  * Selezionare il **+** pulsante per creare un nuovo **classe utente**. Accettare il tipo (**WebApp1User** se il nome assegnato al progetto **WebApp1**) > **Aggiungi**.
* Selezionare **aggiungere**.

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

Se il scaffolder ASP.NET non si è precedentemente installato, installarlo ora:

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Aggiungere un riferimento pacchetto [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) al file di progetto (con estensione csproj). Eseguire il comando seguente nella directory del progetto:

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Eseguire il comando seguente per elencare le opzioni di scaffolder identità:

```cli
dotnet aspnet-codegenerator identity -h
```

Nella cartella del progetto, eseguire il scaffolder identità:

```cli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

-------------

Seguire le istruzioni disponibili nel [migrazioni, UseAuthentication e layout](xref:security/authentication/scaffold-identity#efm) per eseguire la procedura seguente:

* Creare una migrazione e aggiornare il database.
* Aggiungere `UseAuthentication` a `Startup.Configure`.
* Aggiungere `<partial name="_LoginPartial" />` al file di layout.
* Eseguire il test dell'app:
  * Registrare un utente
  * Selezionare il nuovo nome utente (accanto ai **Logout** collegamento). Potrebbe essere necessario espandere la finestra oppure selezionare l'icona della barra di navigazione per visualizzare il nome utente e altri collegamenti.
  * Selezionare il **i dati personali** scheda.
  * Selezionare il **scaricare** pulsante ed esaminare i *PersonalData.json* file.
  * Test di **eliminare** pulsante, che elimina usato per l'accesso utente.

## <a name="add-custom-user-data-to-the-identity-db"></a>Aggiungere dati dell'utente personalizzati per il database di identità

Aggiornamento di `IdentityUser` derivata con le proprietà personalizzate. Se il nome assegnato al progetto WebApp1, il file è denominato *Areas/Identity/Data/WebApp1User.cs*. Aggiornare il file con il codice seguente:

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Data/WebApp1User.cs)]

Proprietà decorata con il [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attributo sono:

* Eliminate quando si sceglie il *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor pagina chiama `UserManager.Delete`.
* Inclusi i dati scaricati dal *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* pagina Razor.

### <a name="update-the-accountmanageindexcshtml-page"></a>Aggiornare la pagina Account/Manage/Index.cshtml

Aggiornamento di `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* con il seguente codice evidenziato:

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,87-95,120)]

Aggiornamento di *Areas/Identity/Pages/Account/Manage/Index.cshtml* con il markup evidenziato seguente:

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=34-41)]

### <a name="update-the-accountregistercshtml-page"></a>Aggiornare la pagina cshtml ne

Aggiornamento di `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* con il seguente codice evidenziato:

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=8-16,43,44)]

Aggiornamento di *Areas/Identity/Pages/Account/Register.cshtml* con il markup evidenziato seguente:

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

Compilare il progetto.

### <a name="add-a-migration-for-the-custom-user-data"></a>Aggiungere una migrazione per i dati utente personalizzata

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

In Visual Studio **Console di gestione pacchetti**:

```PMC
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli)

```cli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

------

## <a name="test-create-view-download-delete-custom-user-data"></a>Test creare, visualizzare, scaricare, eliminare i dati utente personalizzata

Eseguire il test dell'app:

* Registrare un nuovo utente.
* Visualizzare i dati utente personalizzato nel `/Identity/Account/Manage` pagina.
* Scaricare e visualizzare i dati personali degli utenti dal `/Identity/Account/Manage/PersonalData` pagina.
