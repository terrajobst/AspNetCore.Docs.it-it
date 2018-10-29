---
title: Aggiungere, scaricare ed eliminare dati utente personalizzati all'identità in un progetto ASP.NET Core
author: rick-anderson
description: Informazioni su come aggiungere dati utente personalizzati all'identità in un progetto ASP.NET Core. Eliminare i dati al GDPR.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 6/16/2018
uid: security/authentication/add-user-data
ms.openlocfilehash: bc9d46f74a4bc270ffe5cbed16085e0a8043b35c
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206822"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a><span data-ttu-id="c91cd-104">Aggiungere, scaricare ed eliminare dati utente personalizzati all'identità in un progetto ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c91cd-104">Add, download, and delete custom user data to Identity in an ASP.NET Core project</span></span>

<span data-ttu-id="c91cd-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c91cd-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c91cd-106">Questo articolo illustra come:</span><span class="sxs-lookup"><span data-stu-id="c91cd-106">This article shows how to:</span></span>

* <span data-ttu-id="c91cd-107">Aggiungere dati utente personalizzati per un'app web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c91cd-107">Add custom user data to an ASP.NET Core web app.</span></span>
* <span data-ttu-id="c91cd-108">Complementare il modello di dati utente personalizzati con il [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) pertanto è automaticamente disponibile per il download e l'eliminazione dell'attributo.</span><span class="sxs-lookup"><span data-stu-id="c91cd-108">Decorate the custom user data model with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribute so it's automatically available for download and deletion.</span></span> <span data-ttu-id="c91cd-109">Rendere i dati in grado di essere scaricati ed eliminato aiuta a soddisfare [GDPR](xref:security/gdpr) requisiti.</span><span class="sxs-lookup"><span data-stu-id="c91cd-109">Making the data able to be downloaded and deleted helps meet [GDPR](xref:security/gdpr) requirements.</span></span>

<span data-ttu-id="c91cd-110">Esempio di progetto viene creato da un'app web Razor Pages, ma le istruzioni sono simili per un'app web ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="c91cd-110">The project sample is created from a Razor Pages web app, but the instructions are similar for a ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="c91cd-111">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([procedura per il download](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c91cd-111">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c91cd-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="c91cd-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="c91cd-113">Creare un'app Web Razor</span><span class="sxs-lookup"><span data-stu-id="c91cd-113">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c91cd-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c91cd-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c91cd-115">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="c91cd-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="c91cd-116">Denominare il progetto **App Web 1** se si desidera corrispondere lo spazio dei nomi le [Scarica esempio](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) codice.</span><span class="sxs-lookup"><span data-stu-id="c91cd-116">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) code.</span></span>
* <span data-ttu-id="c91cd-117">Selezionare **applicazione Web ASP.NET Core** > **OK**</span><span class="sxs-lookup"><span data-stu-id="c91cd-117">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="c91cd-118">Selezionare **ASP.NET Core 2.1** nell'elenco a discesa</span><span class="sxs-lookup"><span data-stu-id="c91cd-118">Select **ASP.NET Core 2.1** in the dropdown</span></span>
* <span data-ttu-id="c91cd-119">Selezionare **applicazione Web**  > **OK**</span><span class="sxs-lookup"><span data-stu-id="c91cd-119">Select **Web Application**  > **OK**</span></span>
* <span data-ttu-id="c91cd-120">Compilare ed eseguire il progetto.</span><span class="sxs-lookup"><span data-stu-id="c91cd-120">Build and run the project.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c91cd-121">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="c91cd-121">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a><span data-ttu-id="c91cd-122">Eseguire l'utilità di scaffolding di identità</span><span class="sxs-lookup"><span data-stu-id="c91cd-122">Run the Identity scaffolder</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c91cd-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c91cd-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c91cd-124">Dal **Esplora soluzioni**, fare doppio clic sul progetto > **Add** > **nuovo elemento di scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="c91cd-124">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="c91cd-125">Dal riquadro sinistro della finestra di **Add Scaffold** finestra di dialogo, seleziona **identità** > **aggiungere**.</span><span class="sxs-lookup"><span data-stu-id="c91cd-125">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="c91cd-126">Nel **identità ADD** finestra di dialogo, le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="c91cd-126">In the **ADD Identity** dialog, the following options:</span></span>
  * <span data-ttu-id="c91cd-127">Selezionare il file di layout esistente *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c91cd-127">Select the existing layout  file  *~/Pages/Shared/_Layout.cshtml*</span></span>
  * <span data-ttu-id="c91cd-128">Selezionare i file seguenti per eseguire l'override:</span><span class="sxs-lookup"><span data-stu-id="c91cd-128">Select the following files to override:</span></span>
    * <span data-ttu-id="c91cd-129">**Account/Register**</span><span class="sxs-lookup"><span data-stu-id="c91cd-129">**Account/Register**</span></span>
    * <span data-ttu-id="c91cd-130">**Account/gestire/indice**</span><span class="sxs-lookup"><span data-stu-id="c91cd-130">**Account/Manage/Index**</span></span>
  * <span data-ttu-id="c91cd-131">Selezionare il **+** per creare un nuovo pulsante **classe contesto dati**.</span><span class="sxs-lookup"><span data-stu-id="c91cd-131">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="c91cd-132">Accettare il tipo (**WebApp1.Models.WebApp1Context** se il progetto viene denominato **App Web 1**).</span><span class="sxs-lookup"><span data-stu-id="c91cd-132">Accept the type (**WebApp1.Models.WebApp1Context** if the project is named **WebApp1**).</span></span>
  * <span data-ttu-id="c91cd-133">Selezionare il **+** per creare un nuovo pulsante **classe utente**.</span><span class="sxs-lookup"><span data-stu-id="c91cd-133">Select the **+** button to create a new **User class**.</span></span> <span data-ttu-id="c91cd-134">Accettare il tipo (**WebApp1User** se il progetto viene denominato **App Web 1**) > **Add**.</span><span class="sxs-lookup"><span data-stu-id="c91cd-134">Accept the type (**WebApp1User** if the project is named **WebApp1**) > **Add**.</span></span>
* <span data-ttu-id="c91cd-135">Selezionare **aggiungere**.</span><span class="sxs-lookup"><span data-stu-id="c91cd-135">Select **ADD**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c91cd-136">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="c91cd-136">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="c91cd-137">Se l'utilità di scaffolding di ASP.NET Core non è stato precedentemente installato, installarlo ora:</span><span class="sxs-lookup"><span data-stu-id="c91cd-137">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="c91cd-138">Aggiungere un riferimento al pacchetto [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) al file di progetto (con estensione csproj).</span><span class="sxs-lookup"><span data-stu-id="c91cd-138">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (.csproj) file.</span></span> <span data-ttu-id="c91cd-139">Eseguire il comando seguente nella directory del progetto:</span><span class="sxs-lookup"><span data-stu-id="c91cd-139">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="c91cd-140">Eseguire il comando seguente per elencare le opzioni di utilità di scaffolding di identità:</span><span class="sxs-lookup"><span data-stu-id="c91cd-140">Run the following command to list the Identity scaffolder options:</span></span>

```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="c91cd-141">Nella cartella del progetto, eseguire l'utilità di scaffolding di identità:</span><span class="sxs-lookup"><span data-stu-id="c91cd-141">In the project folder, run the Identity scaffolder:</span></span>

```cli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

-------------

<span data-ttu-id="c91cd-142">Seguire le istruzioni disponibili nel [migrazioni UseAuthentication e layout](xref:security/authentication/scaffold-identity#efm) per eseguire la procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="c91cd-142">Follow the instruction in [Migrations, UseAuthentication, and layout](xref:security/authentication/scaffold-identity#efm) to perform the following steps:</span></span>

* <span data-ttu-id="c91cd-143">Creare una migrazione e aggiornare il database.</span><span class="sxs-lookup"><span data-stu-id="c91cd-143">Create a migration and update the database.</span></span>
* <span data-ttu-id="c91cd-144">Aggiungere `UseAuthentication` a `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="c91cd-144">Add `UseAuthentication` to `Startup.Configure`.</span></span>
* <span data-ttu-id="c91cd-145">Aggiungere `<partial name="_LoginPartial" />` nel file di layout.</span><span class="sxs-lookup"><span data-stu-id="c91cd-145">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>
* <span data-ttu-id="c91cd-146">Eseguire il test dell'app:</span><span class="sxs-lookup"><span data-stu-id="c91cd-146">Test the app:</span></span>
  * <span data-ttu-id="c91cd-147">Registrare un utente</span><span class="sxs-lookup"><span data-stu-id="c91cd-147">Register a user</span></span>
  * <span data-ttu-id="c91cd-148">Selezionare il nuovo nome utente (accanto al **Logout** collegamento).</span><span class="sxs-lookup"><span data-stu-id="c91cd-148">Select the new user name (next to the **Logout** link).</span></span> <span data-ttu-id="c91cd-149">Potrebbe essere necessario espandere la finestra oppure selezionare l'icona della barra di navigazione per visualizzare il nome utente e altri collegamenti.</span><span class="sxs-lookup"><span data-stu-id="c91cd-149">You might need to expand the window or select the navigation bar icon to show the user name and other links.</span></span>
  * <span data-ttu-id="c91cd-150">Selezionare il **i dati personali** scheda.</span><span class="sxs-lookup"><span data-stu-id="c91cd-150">Select the **Personal Data** tab.</span></span>
  * <span data-ttu-id="c91cd-151">Selezionare il **scaricare** pulsante ed esaminare le *PersonalData.json* file.</span><span class="sxs-lookup"><span data-stu-id="c91cd-151">Select the **Download** button and examined the *PersonalData.json* file.</span></span>
  * <span data-ttu-id="c91cd-152">Test di **eliminare** pulsante che consente di eliminare usato per l'accesso utente.</span><span class="sxs-lookup"><span data-stu-id="c91cd-152">Test the **Delete** button, which deletes the logged on user.</span></span>

## <a name="add-custom-user-data-to-the-identity-db"></a><span data-ttu-id="c91cd-153">Aggiungere dati utente personalizzati per il database di identità</span><span class="sxs-lookup"><span data-stu-id="c91cd-153">Add custom user data to the Identity DB</span></span>

<span data-ttu-id="c91cd-154">Aggiornamento di `IdentityUser` con proprietà personalizzate della classe derivata.</span><span class="sxs-lookup"><span data-stu-id="c91cd-154">Update the `IdentityUser` derived class with custom properties.</span></span> <span data-ttu-id="c91cd-155">Se il progetto di App Web 1 denominata, il file è denominato *Areas/Identity/Data/WebApp1User.cs*.</span><span class="sxs-lookup"><span data-stu-id="c91cd-155">If you named the project WebApp1, the file is named *Areas/Identity/Data/WebApp1User.cs*.</span></span> <span data-ttu-id="c91cd-156">Aggiornare il file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="c91cd-156">Update the file with the following code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Data/WebApp1User.cs)]

<span data-ttu-id="c91cd-157">Proprietà decorata con il [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attributo sono:</span><span class="sxs-lookup"><span data-stu-id="c91cd-157">Properties decorated with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribute are:</span></span>

* <span data-ttu-id="c91cd-158">Eliminato quando la *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* pagina Razor chiama `UserManager.Delete`.</span><span class="sxs-lookup"><span data-stu-id="c91cd-158">Deleted when the *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor Page calls `UserManager.Delete`.</span></span>
* <span data-ttu-id="c91cd-159">Incluso nei dati scaricati dal *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="c91cd-159">Included in the downloaded data by the *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span></span>

### <a name="update-the-accountmanageindexcshtml-page"></a><span data-ttu-id="c91cd-160">Aggiornare la pagina Account/Manage/Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="c91cd-160">Update the Account/Manage/Index.cshtml page</span></span>

<span data-ttu-id="c91cd-161">Aggiorna il `InputModel` nelle *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* con il seguente codice evidenziato:</span><span class="sxs-lookup"><span data-stu-id="c91cd-161">Update the `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,87-95,120)]

<span data-ttu-id="c91cd-162">Aggiorna il *Areas/Identity/Pages/Account/Manage/Index.cshtml* con il markup evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="c91cd-162">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=34-41)]

### <a name="update-the-accountregistercshtml-page"></a><span data-ttu-id="c91cd-163">Aggiornare la pagina Register</span><span class="sxs-lookup"><span data-stu-id="c91cd-163">Update the Account/Register.cshtml page</span></span>

<span data-ttu-id="c91cd-164">Aggiorna il `InputModel` nelle *Areas/Identity/Pages/Account/Register.cshtml.cs* con il seguente codice evidenziato:</span><span class="sxs-lookup"><span data-stu-id="c91cd-164">Update the `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=8-16,43,44)]

<span data-ttu-id="c91cd-165">Aggiorna il *Areas/Identity/Pages/Account/Register.cshtml* con il markup evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="c91cd-165">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

<span data-ttu-id="c91cd-166">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="c91cd-166">Build the project.</span></span>

### <a name="add-a-migration-for-the-custom-user-data"></a><span data-ttu-id="c91cd-167">Aggiungere una migrazione per i dati utente personalizzati</span><span class="sxs-lookup"><span data-stu-id="c91cd-167">Add a migration for the custom user data</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c91cd-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c91cd-168">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c91cd-169">In Visual Studio **Console di gestione pacchetti**:</span><span class="sxs-lookup"><span data-stu-id="c91cd-169">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c91cd-170">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="c91cd-170">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

------

## <a name="test-create-view-download-delete-custom-user-data"></a><span data-ttu-id="c91cd-171">Test creare, visualizzare, scaricare ed eliminare i dati utente personalizzati</span><span class="sxs-lookup"><span data-stu-id="c91cd-171">Test create, view, download, delete custom user data</span></span>

<span data-ttu-id="c91cd-172">Eseguire il test dell'app:</span><span class="sxs-lookup"><span data-stu-id="c91cd-172">Test the app:</span></span>

* <span data-ttu-id="c91cd-173">Registrare un nuovo utente.</span><span class="sxs-lookup"><span data-stu-id="c91cd-173">Register a new user.</span></span>
* <span data-ttu-id="c91cd-174">Visualizzare i dati utente personalizzati nel `/Identity/Account/Manage` pagina.</span><span class="sxs-lookup"><span data-stu-id="c91cd-174">View the custom user data on the `/Identity/Account/Manage` page.</span></span>
* <span data-ttu-id="c91cd-175">Scaricare e visualizzare i dati personali degli utenti dal `/Identity/Account/Manage/PersonalData` pagina.</span><span class="sxs-lookup"><span data-stu-id="c91cd-175">Download and view the users personal data from the `/Identity/Account/Manage/PersonalData` page.</span></span>
