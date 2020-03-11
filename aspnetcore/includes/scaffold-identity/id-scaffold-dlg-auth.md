::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="70309-101">Eseguire l'impalcatura delle identità:</span><span class="sxs-lookup"><span data-stu-id="70309-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="70309-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="70309-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="70309-103">Da **Esplora soluzioni**, fare clic con il pulsante destro del mouse sul progetto > **Aggiungi** > **nuovo elemento con impalcatura**.</span><span class="sxs-lookup"><span data-stu-id="70309-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="70309-104">Dal riquadro sinistro della finestra di dialogo **Aggiungi impalcatura** selezionare **Identity** > **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="70309-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="70309-105">Nella finestra di dialogo **Aggiungi identità** selezionare le opzioni desiderate.</span><span class="sxs-lookup"><span data-stu-id="70309-105">In the **Add Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="70309-106">Selezionare la pagina layout esistente. in alternativa, il file di layout verrà sovrascritto con un markup errato.</span><span class="sxs-lookup"><span data-stu-id="70309-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="70309-107">Quando si seleziona un file *\_layout. cshtml* esistente, questo **non** viene sovrascritto.</span><span class="sxs-lookup"><span data-stu-id="70309-107">When an existing *\_Layout.cshtml* file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="70309-108">Ad esempio: `~/Pages/Shared/_Layout.cshtml` per Razor Pages `~/Views/Shared/_Layout.cshtml` per i progetti MVC</span><span class="sxs-lookup"><span data-stu-id="70309-108">For example: `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
* <span data-ttu-id="70309-109">Per usare il contesto dei dati esistente, selezionare almeno un file di cui eseguire l'override.</span><span class="sxs-lookup"><span data-stu-id="70309-109">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="70309-110">È necessario selezionare almeno un file per aggiungere il contesto dei dati.</span><span class="sxs-lookup"><span data-stu-id="70309-110">You must select at least one file to add your data context.</span></span>
  * <span data-ttu-id="70309-111">Selezionare la classe del contesto dati.</span><span class="sxs-lookup"><span data-stu-id="70309-111">Select your data context class.</span></span>
  * <span data-ttu-id="70309-112">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="70309-112">Select **Add**.</span></span>
* <span data-ttu-id="70309-113">Per creare un nuovo contesto utente ed eventualmente creare una classe utente personalizzata per l'identità:</span><span class="sxs-lookup"><span data-stu-id="70309-113">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="70309-114">Selezionare il pulsante **+** per creare una nuova **classe del contesto dati**.</span><span class="sxs-lookup"><span data-stu-id="70309-114">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="70309-115">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="70309-115">Select **Add**.</span></span>

<span data-ttu-id="70309-116">Nota: se si sta creando un nuovo contesto utente, non è necessario selezionare un file di cui eseguire l'override.</span><span class="sxs-lookup"><span data-stu-id="70309-116">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="70309-117">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="70309-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="70309-118">Se l'utilità di scaffolding di ASP.NET Core non è stato precedentemente installato, installarlo ora:</span><span class="sxs-lookup"><span data-stu-id="70309-118">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="70309-119">Aggiungere i riferimenti ai pacchetti NuGet necessari al file del progetto (\*. csproj).</span><span class="sxs-lookup"><span data-stu-id="70309-119">Add required NuGet package references to the project (\*.csproj) file.</span></span> <span data-ttu-id="70309-120">Eseguire il comando seguente nella directory del progetto:</span><span class="sxs-lookup"><span data-stu-id="70309-120">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="70309-121">Eseguire il comando seguente per elencare le opzioni di utilità di scaffolding di identità:</span><span class="sxs-lookup"><span data-stu-id="70309-121">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

[!INCLUDE[](~/includes/scaffoldTFM.md)]

<span data-ttu-id="70309-122">Nella cartella del progetto eseguire l'impalcatura di identità con le opzioni desiderate.</span><span class="sxs-lookup"><span data-stu-id="70309-122">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="70309-123">Ad esempio, per configurare Identity con l'interfaccia utente predefinita e il numero minimo di file, eseguire il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="70309-123">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="70309-124">Usare il nome completo corretto per il contesto del database:</span><span class="sxs-lookup"><span data-stu-id="70309-124">Use the correct fully qualified name for your DB context:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login"
```

<span data-ttu-id="70309-125">PowerShell usa il punto e virgola come separatore di comandi.</span><span class="sxs-lookup"><span data-stu-id="70309-125">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="70309-126">Quando si usa PowerShell, usare il carattere di escape per i punti e virgola nell'elenco dei file o inserire l'elenco di file tra virgolette doppie.</span><span class="sxs-lookup"><span data-stu-id="70309-126">When using PowerShell, escape the semi-colons in the file list or put the file list in double quotes.</span></span> <span data-ttu-id="70309-127">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="70309-127">For example:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="70309-128">Se si esegue l'impalcatura delle identità senza specificare il flag di `--files` o il flag di `--useDefaultUI`, tutte le pagine dell'interfaccia utente di identità disponibili verranno create nel progetto.</span><span class="sxs-lookup"><span data-stu-id="70309-128">If you run the Identity scaffolder without specifying the `--files` flag or the `--useDefaultUI` flag, all the available Identity UI pages will be created in your project.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="70309-129">Eseguire l'impalcatura delle identità:</span><span class="sxs-lookup"><span data-stu-id="70309-129">Run the Identity scaffolder:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="70309-130">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="70309-130">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="70309-131">Da **Esplora soluzioni**, fare clic con il pulsante destro del mouse sul progetto > **Aggiungi** > **nuovo elemento con impalcatura**.</span><span class="sxs-lookup"><span data-stu-id="70309-131">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="70309-132">Dal riquadro sinistro della finestra di dialogo **Aggiungi impalcatura** selezionare **Identity** > **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="70309-132">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="70309-133">Nella finestra di dialogo **Aggiungi identità** selezionare le opzioni desiderate.</span><span class="sxs-lookup"><span data-stu-id="70309-133">In the **Add Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="70309-134">Selezionare la pagina layout esistente. in alternativa, il file di layout verrà sovrascritto con un markup errato.</span><span class="sxs-lookup"><span data-stu-id="70309-134">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="70309-135">Quando si seleziona un file *\_layout. cshtml* esistente, questo **non** viene sovrascritto.</span><span class="sxs-lookup"><span data-stu-id="70309-135">When an existing *\_Layout.cshtml* file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="70309-136">Ad esempio: `~/Pages/Shared/_Layout.cshtml` per Razor Pages `~/Views/Shared/_Layout.cshtml` per i progetti MVC</span><span class="sxs-lookup"><span data-stu-id="70309-136">For example: `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
* <span data-ttu-id="70309-137">Per usare il contesto dei dati esistente, selezionare almeno un file di cui eseguire l'override.</span><span class="sxs-lookup"><span data-stu-id="70309-137">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="70309-138">È necessario selezionare almeno un file per aggiungere il contesto dei dati.</span><span class="sxs-lookup"><span data-stu-id="70309-138">You must select at least one file to add your data context.</span></span>
  * <span data-ttu-id="70309-139">Selezionare la classe del contesto dati.</span><span class="sxs-lookup"><span data-stu-id="70309-139">Select your data context class.</span></span>
  * <span data-ttu-id="70309-140">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="70309-140">Select **Add**.</span></span>
* <span data-ttu-id="70309-141">Per creare un nuovo contesto utente ed eventualmente creare una classe utente personalizzata per l'identità:</span><span class="sxs-lookup"><span data-stu-id="70309-141">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="70309-142">Selezionare il pulsante **+** per creare una nuova **classe del contesto dati**.</span><span class="sxs-lookup"><span data-stu-id="70309-142">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="70309-143">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="70309-143">Select **Add**.</span></span>

<span data-ttu-id="70309-144">Nota: se si sta creando un nuovo contesto utente, non è necessario selezionare un file di cui eseguire l'override.</span><span class="sxs-lookup"><span data-stu-id="70309-144">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="70309-145">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="70309-145">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="70309-146">Se l'utilità di scaffolding di ASP.NET Core non è stato precedentemente installato, installarlo ora:</span><span class="sxs-lookup"><span data-stu-id="70309-146">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="70309-147">Aggiungere un riferimento al pacchetto [Microsoft. VisualStudio. Web. CodeGeneration. Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) al file del progetto (\*. csproj).</span><span class="sxs-lookup"><span data-stu-id="70309-147">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="70309-148">Eseguire il comando seguente nella directory del progetto:</span><span class="sxs-lookup"><span data-stu-id="70309-148">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="70309-149">Eseguire il comando seguente per elencare le opzioni di utilità di scaffolding di identità:</span><span class="sxs-lookup"><span data-stu-id="70309-149">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="70309-150">Nella cartella del progetto eseguire l'impalcatura di identità con le opzioni desiderate.</span><span class="sxs-lookup"><span data-stu-id="70309-150">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="70309-151">Ad esempio, per configurare Identity con l'interfaccia utente predefinita e il numero minimo di file, eseguire il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="70309-151">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="70309-152">Usare il nome completo corretto per il contesto del database:</span><span class="sxs-lookup"><span data-stu-id="70309-152">Use the correct fully qualified name for your DB context:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login"
```

<span data-ttu-id="70309-153">PowerShell usa il punto e virgola come separatore di comandi.</span><span class="sxs-lookup"><span data-stu-id="70309-153">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="70309-154">Quando si usa PowerShell, usare il carattere di escape per i punti e virgola nell'elenco dei file o inserire l'elenco di file tra virgolette doppie.</span><span class="sxs-lookup"><span data-stu-id="70309-154">When using PowerShell, escape the semi-colons in the file list or put the file list in double quotes.</span></span> <span data-ttu-id="70309-155">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="70309-155">For example:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="70309-156">Se si esegue l'impalcatura delle identità senza specificare il flag di `--files` o il flag di `--useDefaultUI`, tutte le pagine dell'interfaccia utente di identità disponibili verranno create nel progetto.</span><span class="sxs-lookup"><span data-stu-id="70309-156">If you run the Identity scaffolder without specifying the `--files` flag or the `--useDefaultUI` flag, all the available Identity UI pages will be created in your project.</span></span>

---

::: moniker-end