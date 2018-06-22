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
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a><span data-ttu-id="944ed-104">Aggiungere, scaricare ed eliminare dati dell'utente personalizzati di identità in un progetto ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="944ed-104">Add, download, and delete custom user data to Identity in an ASP.NET Core project</span></span>

<span data-ttu-id="944ed-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="944ed-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="944ed-106">Questo articolo illustra come:</span><span class="sxs-lookup"><span data-stu-id="944ed-106">This article shows how to:</span></span>

* <span data-ttu-id="944ed-107">Aggiungere dati dell'utente personalizzati per un'app web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="944ed-107">Add custom user data to an ASP.NET Core web app.</span></span>
* <span data-ttu-id="944ed-108">Decorare il modello di dati utente personalizzata con il [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attributo pertanto è automaticamente disponibile per il download e l'eliminazione.</span><span class="sxs-lookup"><span data-stu-id="944ed-108">Decorate the custom user data model with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribute so it's automatically available for download and deletion.</span></span> <span data-ttu-id="944ed-109">Rendere i dati in grado di essere scaricata ed eliminata consente di soddisfare [PILR](xref:security/gdpr) requisiti.</span><span class="sxs-lookup"><span data-stu-id="944ed-109">Making the data able to be downloaded and deleted helps meet [GDPR](xref:security/gdpr) requirements.</span></span>

<span data-ttu-id="944ed-110">L'esempio di progetto viene creato da un'app web pagine Razor, ma le istruzioni sono simili per un'app web ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="944ed-110">The project sample is created from a Razor Pages web app, but the instructions are similar for a ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="944ed-111">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="944ed-111">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="944ed-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="944ed-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="944ed-113">Creare un'app Web Razor</span><span class="sxs-lookup"><span data-stu-id="944ed-113">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="944ed-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="944ed-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="944ed-115">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="944ed-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="944ed-116">Denominare il progetto **WebApp1** se si desidera corrispondere lo spazio dei nomi il [scaricare esempio](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) codice.</span><span class="sxs-lookup"><span data-stu-id="944ed-116">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) code.</span></span>
* <span data-ttu-id="944ed-117">Selezionare **applicazione Web ASP.NET Core** > **OK**</span><span class="sxs-lookup"><span data-stu-id="944ed-117">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="944ed-118">Selezionare **ASP.NET Core 2.1** nell'elenco a discesa</span><span class="sxs-lookup"><span data-stu-id="944ed-118">Select **ASP.NET Core 2.1** in the dropdown</span></span>
* <span data-ttu-id="944ed-119">Selezionare **applicazione Web**  > **OK**</span><span class="sxs-lookup"><span data-stu-id="944ed-119">Select **Web Application**  > **OK**</span></span>
* <span data-ttu-id="944ed-120">Compilare ed eseguire il progetto.</span><span class="sxs-lookup"><span data-stu-id="944ed-120">Build and run the project.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="944ed-121">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="944ed-121">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp -o WebApp1
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

## <a name="run-the-identity-scaffolder"></a><span data-ttu-id="944ed-122">Eseguire il scaffolder identità</span><span class="sxs-lookup"><span data-stu-id="944ed-122">Run the Identity scaffolder</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="944ed-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="944ed-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="944ed-124">Dal **Esplora soluzioni**, fare clic su progetto > **Add** > **nuovo elemento di scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="944ed-124">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="944ed-125">Nel riquadro sinistro della **aggiungere lo scaffolding** finestra di dialogo, seleziona **identità** > **aggiungere**.</span><span class="sxs-lookup"><span data-stu-id="944ed-125">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="944ed-126">Nel **identità Aggiungi** finestra di dialogo, le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="944ed-126">In the **ADD Identity** dialog, the following options:</span></span>
  * <span data-ttu-id="944ed-127">Selezionare il file di layout esistente *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="944ed-127">Select your existing layout  file  *~/Pages/Shared/_Layout.cshtml*</span></span>
  * <span data-ttu-id="944ed-128">Selezionare i file seguenti per eseguire l'override:</span><span class="sxs-lookup"><span data-stu-id="944ed-128">Select the following files to override:</span></span>
    * <span data-ttu-id="944ed-129">**Account/Register**</span><span class="sxs-lookup"><span data-stu-id="944ed-129">**Account/Register**</span></span>
    * <span data-ttu-id="944ed-130">**Account/gestire/indice**</span><span class="sxs-lookup"><span data-stu-id="944ed-130">**Account/Manage/Index**</span></span>
  * <span data-ttu-id="944ed-131">Selezionare il **+** pulsante per creare un nuovo **classe contesto dati**.</span><span class="sxs-lookup"><span data-stu-id="944ed-131">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="944ed-132">Accettare il tipo (**WebApp1.Models.WebApp1Context** se il nome assegnato al progetto **WebApp1**).</span><span class="sxs-lookup"><span data-stu-id="944ed-132">Accept the type (**WebApp1.Models.WebApp1Context** if you named the project **WebApp1**).</span></span>
  * <span data-ttu-id="944ed-133">Selezionare il **+** pulsante per creare un nuovo **classe utente**.</span><span class="sxs-lookup"><span data-stu-id="944ed-133">Select the **+** button to create a new **User class**.</span></span> <span data-ttu-id="944ed-134">Accettare il tipo (**WebApp1User** se il nome assegnato al progetto **WebApp1**) > **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="944ed-134">Accept the type (**WebApp1User** if you named the project **WebApp1**) > **Add**.</span></span>
* <span data-ttu-id="944ed-135">Selezionare **aggiungere**.</span><span class="sxs-lookup"><span data-stu-id="944ed-135">Select **ADD**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="944ed-136">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="944ed-136">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="944ed-137">Se il scaffolder ASP.NET non si è precedentemente installato, installarlo ora:</span><span class="sxs-lookup"><span data-stu-id="944ed-137">If you have not previously installed the ASP.NET scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="944ed-138">Aggiungere un riferimento pacchetto [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) al file di progetto (con estensione csproj).</span><span class="sxs-lookup"><span data-stu-id="944ed-138">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (.csproj) file.</span></span> <span data-ttu-id="944ed-139">Eseguire il comando seguente nella directory del progetto:</span><span class="sxs-lookup"><span data-stu-id="944ed-139">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="944ed-140">Eseguire il comando seguente per elencare le opzioni di scaffolder identità:</span><span class="sxs-lookup"><span data-stu-id="944ed-140">Run the following command to list the Identity scaffolder options:</span></span>

```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="944ed-141">Nella cartella del progetto, eseguire il scaffolder identità:</span><span class="sxs-lookup"><span data-stu-id="944ed-141">In the project folder, run the Identity scaffolder:</span></span>

```cli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

-------------

<span data-ttu-id="944ed-142">Seguire le istruzioni disponibili nel [migrazioni, UseAuthentication e layout](xref:security/authentication/scaffold-identity#efm) per eseguire la procedura seguente:</span><span class="sxs-lookup"><span data-stu-id="944ed-142">Follow the instruction in [Migrations, UseAuthentication, and layout](xref:security/authentication/scaffold-identity#efm) to perform the following steps:</span></span>

* <span data-ttu-id="944ed-143">Creare una migrazione e aggiornare il database.</span><span class="sxs-lookup"><span data-stu-id="944ed-143">Create a migration and update the database.</span></span>
* <span data-ttu-id="944ed-144">Aggiungere `UseAuthentication` a `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="944ed-144">Add `UseAuthentication` to `Startup.Configure`.</span></span>
* <span data-ttu-id="944ed-145">Aggiungere `<partial name="_LoginPartial" />` al file di layout.</span><span class="sxs-lookup"><span data-stu-id="944ed-145">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>
* <span data-ttu-id="944ed-146">Eseguire il test dell'app:</span><span class="sxs-lookup"><span data-stu-id="944ed-146">Test the app:</span></span>
  * <span data-ttu-id="944ed-147">Registrare un utente</span><span class="sxs-lookup"><span data-stu-id="944ed-147">Register a user</span></span>
  * <span data-ttu-id="944ed-148">Selezionare il nuovo nome utente (accanto ai **Logout** collegamento).</span><span class="sxs-lookup"><span data-stu-id="944ed-148">Select the new user name (next to the **Logout** link).</span></span> <span data-ttu-id="944ed-149">Potrebbe essere necessario espandere la finestra oppure selezionare l'icona della barra di navigazione per visualizzare il nome utente e altri collegamenti.</span><span class="sxs-lookup"><span data-stu-id="944ed-149">You might need to expand the window or select the navigation bar icon to show the user name and other links.</span></span>
  * <span data-ttu-id="944ed-150">Selezionare il **i dati personali** scheda.</span><span class="sxs-lookup"><span data-stu-id="944ed-150">Select the **Personal Data** tab.</span></span>
  * <span data-ttu-id="944ed-151">Selezionare il **scaricare** pulsante ed esaminare i *PersonalData.json* file.</span><span class="sxs-lookup"><span data-stu-id="944ed-151">Select the **Download** button and examined the *PersonalData.json* file.</span></span>
  * <span data-ttu-id="944ed-152">Test di **eliminare** pulsante, che elimina usato per l'accesso utente.</span><span class="sxs-lookup"><span data-stu-id="944ed-152">Test the **Delete** button, which deletes the logged on user.</span></span>

## <a name="add-custom-user-data-to-the-identity-db"></a><span data-ttu-id="944ed-153">Aggiungere dati dell'utente personalizzati per il database di identità</span><span class="sxs-lookup"><span data-stu-id="944ed-153">Add custom user data to the Identity DB</span></span>

<span data-ttu-id="944ed-154">Aggiornamento di `IdentityUser` derivata con le proprietà personalizzate.</span><span class="sxs-lookup"><span data-stu-id="944ed-154">Update the `IdentityUser` derived class with custom properties.</span></span> <span data-ttu-id="944ed-155">Se il nome assegnato al progetto WebApp1, il file è denominato *Areas/Identity/Data/WebApp1User.cs*.</span><span class="sxs-lookup"><span data-stu-id="944ed-155">If you named your project WebApp1, the file is named *Areas/Identity/Data/WebApp1User.cs*.</span></span> <span data-ttu-id="944ed-156">Aggiornare il file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="944ed-156">Update the file with the following code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Data/WebApp1User.cs)]

<span data-ttu-id="944ed-157">Proprietà decorata con il [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attributo sono:</span><span class="sxs-lookup"><span data-stu-id="944ed-157">Properties decorated with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribute are:</span></span>

* <span data-ttu-id="944ed-158">Eliminate quando si sceglie il *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor pagina chiama `UserManager.Delete`.</span><span class="sxs-lookup"><span data-stu-id="944ed-158">Deleted when the *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor Page calls `UserManager.Delete`.</span></span>
* <span data-ttu-id="944ed-159">Inclusi i dati scaricati dal *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* pagina Razor.</span><span class="sxs-lookup"><span data-stu-id="944ed-159">Included in the downloaded data by the *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span></span>

### <a name="update-the-accountmanageindexcshtml-page"></a><span data-ttu-id="944ed-160">Aggiornare la pagina Account/Manage/Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="944ed-160">Update the Account/Manage/Index.cshtml page</span></span>

<span data-ttu-id="944ed-161">Aggiornamento di `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* con il seguente codice evidenziato:</span><span class="sxs-lookup"><span data-stu-id="944ed-161">Update the `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,87-95,120)]

<span data-ttu-id="944ed-162">Aggiornamento di *Areas/Identity/Pages/Account/Manage/Index.cshtml* con il markup evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="944ed-162">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=34-41)]

### <a name="update-the-accountregistercshtml-page"></a><span data-ttu-id="944ed-163">Aggiornare la pagina cshtml ne</span><span class="sxs-lookup"><span data-stu-id="944ed-163">Update the Account/Register.cshtml page</span></span>

<span data-ttu-id="944ed-164">Aggiornamento di `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* con il seguente codice evidenziato:</span><span class="sxs-lookup"><span data-stu-id="944ed-164">Update the `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=8-16,43,44)]

<span data-ttu-id="944ed-165">Aggiornamento di *Areas/Identity/Pages/Account/Register.cshtml* con il markup evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="944ed-165">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

<span data-ttu-id="944ed-166">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="944ed-166">Build the project.</span></span>

### <a name="add-a-migration-for-the-custom-user-data"></a><span data-ttu-id="944ed-167">Aggiungere una migrazione per i dati utente personalizzata</span><span class="sxs-lookup"><span data-stu-id="944ed-167">Add a migration for the custom user data</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="944ed-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="944ed-168">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="944ed-169">In Visual Studio **Console di gestione pacchetti**:</span><span class="sxs-lookup"><span data-stu-id="944ed-169">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="944ed-170">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="944ed-170">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

------

## <a name="test-create-view-download-delete-custom-user-data"></a><span data-ttu-id="944ed-171">Test creare, visualizzare, scaricare, eliminare i dati utente personalizzata</span><span class="sxs-lookup"><span data-stu-id="944ed-171">Test create, view, download, delete custom user data</span></span>

<span data-ttu-id="944ed-172">Eseguire il test dell'app:</span><span class="sxs-lookup"><span data-stu-id="944ed-172">Test the app:</span></span>

* <span data-ttu-id="944ed-173">Registrare un nuovo utente.</span><span class="sxs-lookup"><span data-stu-id="944ed-173">Register a new user.</span></span>
* <span data-ttu-id="944ed-174">Visualizzare i dati utente personalizzato nel `/Identity/Account/Manage` pagina.</span><span class="sxs-lookup"><span data-stu-id="944ed-174">View the custom user data on the `/Identity/Account/Manage` page.</span></span>
* <span data-ttu-id="944ed-175">Scaricare e visualizzare i dati personali degli utenti dal `/Identity/Account/Manage/PersonalData` pagina.</span><span class="sxs-lookup"><span data-stu-id="944ed-175">Download and view the users personal data from the `/Identity/Account/Manage/PersonalData` page.</span></span>
