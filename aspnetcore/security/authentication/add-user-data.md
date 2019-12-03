---
title: Aggiungere, scaricare ed eliminare dati utente nell'identità in un progetto ASP.NET Core
author: rick-anderson
description: Informazioni su come aggiungere dati utente personalizzati all'identità in un progetto ASP.NET Core. Eliminare i dati per ogni GDPR.
ms.author: riande
ms.date: 06/18/2019
ms.custom: mvc, seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: 6daca5776930f80eec8d81132b5a5c4d4d5c13ad
ms.sourcegitcommit: 0dd224b2b7efca1fda0041b5c3f45080327033f6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/02/2019
ms.locfileid: "74681162"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a><span data-ttu-id="fead2-104">Aggiungere, scaricare ed eliminare dati utente personalizzati nell'identità in un progetto ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fead2-104">Add, download, and delete custom user data to Identity in an ASP.NET Core project</span></span>

<span data-ttu-id="fead2-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fead2-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fead2-106">Questo articolo illustra come:</span><span class="sxs-lookup"><span data-stu-id="fead2-106">This article shows how to:</span></span>

* <span data-ttu-id="fead2-107">Aggiungere dati utente personalizzati a un'app Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fead2-107">Add custom user data to an ASP.NET Core web app.</span></span>
* <span data-ttu-id="fead2-108">Decorare il modello di dati utente personalizzato con l'attributo <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> in modo che sia disponibile automaticamente per il download e l'eliminazione.</span><span class="sxs-lookup"><span data-stu-id="fead2-108">Decorate the custom user data model with the <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> attribute so it's automatically available for download and deletion.</span></span> <span data-ttu-id="fead2-109">La possibilità di scaricare ed eliminare i dati consente di soddisfare i requisiti di [GDPR](xref:security/gdpr) .</span><span class="sxs-lookup"><span data-stu-id="fead2-109">Making the data able to be downloaded and deleted helps meet [GDPR](xref:security/gdpr) requirements.</span></span>

<span data-ttu-id="fead2-110">Il progetto di esempio viene creato da un'app Web Razor Pages, ma le istruzioni sono simili per un'app Web MVC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fead2-110">The project sample is created from a Razor Pages web app, but the instructions are similar for a ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="fead2-111">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fead2-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fead2-112">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="fead2-112">Prerequisites</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [](~/includes/3.0-SDK.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [](~/includes/2.2-SDK.md)]

::: moniker-end

## <a name="create-a-razor-web-app"></a><span data-ttu-id="fead2-113">Creare un'app Web Razor</span><span class="sxs-lookup"><span data-stu-id="fead2-113">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fead2-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fead2-114">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="fead2-115">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="fead2-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="fead2-116">Denominare il progetto **app Web 1** se si desidera che corrisponda allo spazio dei nomi del codice di [esempio di download](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) .</span><span class="sxs-lookup"><span data-stu-id="fead2-116">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span></span>
* <span data-ttu-id="fead2-117">Selezionare **ASP.NET Core applicazione Web** > **OK**</span><span class="sxs-lookup"><span data-stu-id="fead2-117">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="fead2-118">Selezionare **ASP.NET Core 3,0** nell'elenco a discesa</span><span class="sxs-lookup"><span data-stu-id="fead2-118">Select **ASP.NET Core 3.0** in the dropdown</span></span>
* <span data-ttu-id="fead2-119">Selezionare l' **applicazione Web** > **OK**</span><span class="sxs-lookup"><span data-stu-id="fead2-119">Select **Web Application** > **OK**</span></span>
* <span data-ttu-id="fead2-120">Compilare ed eseguire il progetto.</span><span class="sxs-lookup"><span data-stu-id="fead2-120">Build and run the project.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="fead2-121">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="fead2-121">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="fead2-122">Denominare il progetto **app Web 1** se si desidera che corrisponda allo spazio dei nomi del codice di [esempio di download](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) .</span><span class="sxs-lookup"><span data-stu-id="fead2-122">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span></span>
* <span data-ttu-id="fead2-123">Selezionare **ASP.NET Core applicazione Web** > **OK**</span><span class="sxs-lookup"><span data-stu-id="fead2-123">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="fead2-124">Selezionare **ASP.NET Core 2,2** nell'elenco a discesa</span><span class="sxs-lookup"><span data-stu-id="fead2-124">Select **ASP.NET Core 2.2** in the dropdown</span></span>
* <span data-ttu-id="fead2-125">Selezionare l' **applicazione Web** > **OK**</span><span class="sxs-lookup"><span data-stu-id="fead2-125">Select **Web Application** > **OK**</span></span>
* <span data-ttu-id="fead2-126">Compilare ed eseguire il progetto.</span><span class="sxs-lookup"><span data-stu-id="fead2-126">Build and run the project.</span></span>

::: moniker-end


# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="fead2-127">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="fead2-127">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a><span data-ttu-id="fead2-128">Eseguire l'impalcatura delle identità</span><span class="sxs-lookup"><span data-stu-id="fead2-128">Run the Identity scaffolder</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fead2-129">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fead2-129">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="fead2-130">Da **Esplora soluzioni**, fare clic con il pulsante destro del mouse sul progetto > **Aggiungi** > **nuovo elemento con impalcatura**.</span><span class="sxs-lookup"><span data-stu-id="fead2-130">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="fead2-131">Dal riquadro sinistro della finestra di dialogo **Aggiungi impalcatura** selezionare **Identity** > **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="fead2-131">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="fead2-132">Nella finestra di dialogo **Aggiungi identità** sono disponibili le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="fead2-132">In the **ADD Identity** dialog, the following options:</span></span>
  * <span data-ttu-id="fead2-133">Selezionare il file di layout esistente *~/Pages/Shared/_Layout. cshtml*</span><span class="sxs-lookup"><span data-stu-id="fead2-133">Select the existing layout  file  *~/Pages/Shared/_Layout.cshtml*</span></span>
  * <span data-ttu-id="fead2-134">Selezionare i seguenti file di cui eseguire l'override:</span><span class="sxs-lookup"><span data-stu-id="fead2-134">Select the following files to override:</span></span>
    * <span data-ttu-id="fead2-135">**Account/registro**</span><span class="sxs-lookup"><span data-stu-id="fead2-135">**Account/Register**</span></span>
    * <span data-ttu-id="fead2-136">**Account/Gestione/indice**</span><span class="sxs-lookup"><span data-stu-id="fead2-136">**Account/Manage/Index**</span></span>
  * <span data-ttu-id="fead2-137">Selezionare il pulsante **+** per creare una nuova **classe del contesto dati**.</span><span class="sxs-lookup"><span data-stu-id="fead2-137">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="fead2-138">Accettare il tipo (**app Web 1. Models. WebApp1Context** se il progetto è denominato **app Web 1**).</span><span class="sxs-lookup"><span data-stu-id="fead2-138">Accept the type (**WebApp1.Models.WebApp1Context** if the project is named **WebApp1**).</span></span>
  * <span data-ttu-id="fead2-139">Selezionare il pulsante **+** per creare una nuova **classe utente**.</span><span class="sxs-lookup"><span data-stu-id="fead2-139">Select the **+** button to create a new **User class**.</span></span> <span data-ttu-id="fead2-140">Accettare il tipo (**WebApp1User** se il progetto è denominato **app Web 1**) > **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="fead2-140">Accept the type (**WebApp1User** if the project is named **WebApp1**) > **Add**.</span></span>
* <span data-ttu-id="fead2-141">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="fead2-141">Select **ADD**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="fead2-142">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="fead2-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="fead2-143">Se in precedenza non è stato installato il ASP.NET Core impalcatura, installarlo ora:</span><span class="sxs-lookup"><span data-stu-id="fead2-143">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="fead2-144">Aggiungere un riferimento al pacchetto [Microsoft. VisualStudio. Web. CodeGeneration. Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) al file di progetto (con estensione csproj).</span><span class="sxs-lookup"><span data-stu-id="fead2-144">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (.csproj) file.</span></span> <span data-ttu-id="fead2-145">Eseguire il comando seguente nella directory del progetto:</span><span class="sxs-lookup"><span data-stu-id="fead2-145">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="fead2-146">Eseguire il comando seguente per elencare le opzioni dell'impalcatura di identità:</span><span class="sxs-lookup"><span data-stu-id="fead2-146">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="fead2-147">Nella cartella del progetto eseguire l'impalcatura di identità:</span><span class="sxs-lookup"><span data-stu-id="fead2-147">In the project folder, run the Identity scaffolder:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

<span data-ttu-id="fead2-148">Seguire le istruzioni in [Migrations, UseAuthentication e layout](xref:security/authentication/scaffold-identity#efm) per eseguire la procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="fead2-148">Follow the instruction in [Migrations, UseAuthentication, and layout](xref:security/authentication/scaffold-identity#efm) to perform the following steps:</span></span>

* <span data-ttu-id="fead2-149">Creare una migrazione e aggiornare il database.</span><span class="sxs-lookup"><span data-stu-id="fead2-149">Create a migration and update the database.</span></span>
* <span data-ttu-id="fead2-150">Aggiungere `UseAuthentication` a `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="fead2-150">Add `UseAuthentication` to `Startup.Configure`.</span></span>
* <span data-ttu-id="fead2-151">Aggiungere `<partial name="_LoginPartial" />` al file di layout.</span><span class="sxs-lookup"><span data-stu-id="fead2-151">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>
* <span data-ttu-id="fead2-152">Eseguire il test dell'app:</span><span class="sxs-lookup"><span data-stu-id="fead2-152">Test the app:</span></span>
  * <span data-ttu-id="fead2-153">Registrare un utente</span><span class="sxs-lookup"><span data-stu-id="fead2-153">Register a user</span></span>
  * <span data-ttu-id="fead2-154">Selezionare il nuovo nome utente (accanto al collegamento di **disconnessione** ).</span><span class="sxs-lookup"><span data-stu-id="fead2-154">Select the new user name (next to the **Logout** link).</span></span> <span data-ttu-id="fead2-155">Potrebbe essere necessario espandere la finestra o selezionare l'icona della barra di spostamento per visualizzare il nome utente e altri collegamenti.</span><span class="sxs-lookup"><span data-stu-id="fead2-155">You might need to expand the window or select the navigation bar icon to show the user name and other links.</span></span>
  * <span data-ttu-id="fead2-156">Selezionare la scheda **dati personali** .</span><span class="sxs-lookup"><span data-stu-id="fead2-156">Select the **Personal Data** tab.</span></span>
  * <span data-ttu-id="fead2-157">Selezionare il pulsante **download** ed esaminare il file *PersonalData. JSON* .</span><span class="sxs-lookup"><span data-stu-id="fead2-157">Select the **Download** button and examined the *PersonalData.json* file.</span></span>
  * <span data-ttu-id="fead2-158">Testare il pulsante **Elimina per eliminare** l'utente che ha eseguito l'accesso.</span><span class="sxs-lookup"><span data-stu-id="fead2-158">Test the **Delete** button, which deletes the logged on user.</span></span>

## <a name="add-custom-user-data-to-the-identity-db"></a><span data-ttu-id="fead2-159">Aggiungere dati utente personalizzati al database di identità</span><span class="sxs-lookup"><span data-stu-id="fead2-159">Add custom user data to the Identity DB</span></span>

<span data-ttu-id="fead2-160">Aggiornare la classe derivata `IdentityUser` con proprietà personalizzate.</span><span class="sxs-lookup"><span data-stu-id="fead2-160">Update the `IdentityUser` derived class with custom properties.</span></span> <span data-ttu-id="fead2-161">Se è stato denominato il progetto app Web 1, il file è denominato *areas/Identity/data/WebApp1User. cs*.</span><span class="sxs-lookup"><span data-stu-id="fead2-161">If you named the project WebApp1, the file is named *Areas/Identity/Data/WebApp1User.cs*.</span></span> <span data-ttu-id="fead2-162">Aggiornare il file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="fead2-162">Update the file with the following code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

<span data-ttu-id="fead2-163">Le proprietà decorate con l'attributo [personaldata](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) sono:</span><span class="sxs-lookup"><span data-stu-id="fead2-163">Properties decorated with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) attribute are:</span></span>

* <span data-ttu-id="fead2-164">Eliminato quando la pagina Razor *areas/Identity/Pages/account/Manage/DeletePersonalData. cshtml* chiama `UserManager.Delete`.</span><span class="sxs-lookup"><span data-stu-id="fead2-164">Deleted when the *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor Page calls `UserManager.Delete`.</span></span>
* <span data-ttu-id="fead2-165">Incluso nei dati scaricati dalla pagina Razor *areas/Identity/Pages/account/Manage/DownloadPersonalData. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="fead2-165">Included in the downloaded data by the *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span></span>

### <a name="update-the-accountmanageindexcshtml-page"></a><span data-ttu-id="fead2-166">Aggiornare la pagina account/Manage/index. cshtml</span><span class="sxs-lookup"><span data-stu-id="fead2-166">Update the Account/Manage/Index.cshtml page</span></span>

<span data-ttu-id="fead2-167">Aggiornare il `InputModel` in *areas/Identity/Pages/account/Manage/index. cshtml. cs* con il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="fead2-167">Update the `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* with the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=24-32,48-49,96-104,106)]

<span data-ttu-id="fead2-168">Aggiornare le *aree/Identity/Pages/account/Manage/index. cshtml* con il markup evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="fead2-168">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=18-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

<span data-ttu-id="fead2-169">Aggiornare le *aree/Identity/Pages/account/Manage/index. cshtml* con il markup evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="fead2-169">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

::: moniker-end

### <a name="update-the-accountregistercshtml-page"></a><span data-ttu-id="fead2-170">Aggiornare la pagina account/Register. cshtml</span><span class="sxs-lookup"><span data-stu-id="fead2-170">Update the Account/Register.cshtml page</span></span>

<span data-ttu-id="fead2-171">Aggiornare il `InputModel` in *areas/Identity/Pages/account/Register. cshtml. cs* con il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="fead2-171">Update the `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* with the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=30-38,70-71)]

<span data-ttu-id="fead2-172">Aggiornare le *aree/Identity/Pages/account/Register. cshtml* con il markup evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="fead2-172">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

<span data-ttu-id="fead2-173">Aggiornare le *aree/Identity/Pages/account/Register. cshtml* con il markup evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="fead2-173">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end


<span data-ttu-id="fead2-174">Compilazione del progetto.</span><span class="sxs-lookup"><span data-stu-id="fead2-174">Build the project.</span></span>

### <a name="add-a-migration-for-the-custom-user-data"></a><span data-ttu-id="fead2-175">Aggiungere una migrazione per i dati utente personalizzati</span><span class="sxs-lookup"><span data-stu-id="fead2-175">Add a migration for the custom user data</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fead2-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fead2-176">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fead2-177">Nella **console di gestione pacchetti**di Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="fead2-177">In the Visual Studio **Package Manager Console**:</span></span>

```powershell
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="fead2-178">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="fead2-178">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a><span data-ttu-id="fead2-179">Test creare, visualizzare, scaricare, eliminare dati utente personalizzati</span><span class="sxs-lookup"><span data-stu-id="fead2-179">Test create, view, download, delete custom user data</span></span>

<span data-ttu-id="fead2-180">Eseguire il test dell'app:</span><span class="sxs-lookup"><span data-stu-id="fead2-180">Test the app:</span></span>

* <span data-ttu-id="fead2-181">Registrare un nuovo utente.</span><span class="sxs-lookup"><span data-stu-id="fead2-181">Register a new user.</span></span>
* <span data-ttu-id="fead2-182">Visualizzare i dati utente personalizzati nella pagina `/Identity/Account/Manage`.</span><span class="sxs-lookup"><span data-stu-id="fead2-182">View the custom user data on the `/Identity/Account/Manage` page.</span></span>
* <span data-ttu-id="fead2-183">Scaricare e visualizzare i dati personali degli utenti dalla pagina `/Identity/Account/Manage/PersonalData`.</span><span class="sxs-lookup"><span data-stu-id="fead2-183">Download and view the users personal data from the `/Identity/Account/Manage/PersonalData` page.</span></span>
