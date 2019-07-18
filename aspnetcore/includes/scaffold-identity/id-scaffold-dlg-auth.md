<span data-ttu-id="4e14a-101">Eseguire l'impalcatura delle identità:</span><span class="sxs-lookup"><span data-stu-id="4e14a-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4e14a-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4e14a-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="4e14a-103">Dal **Esplora soluzioni**, fare doppio clic sul progetto > **Add** > **nuovo elemento di scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="4e14a-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="4e14a-104">Dal riquadro sinistro della finestra di dialogo **Aggiungi impalcatura** selezionare **Identity** > **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="4e14a-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="4e14a-105">Nella finestra di dialogo **Aggiungi identità** selezionare le opzioni desiderate.</span><span class="sxs-lookup"><span data-stu-id="4e14a-105">In the **Add Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="4e14a-106">Selezionare la pagina layout esistente. in alternativa, il file di layout verrà sovrascritto con un markup errato.</span><span class="sxs-lookup"><span data-stu-id="4e14a-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="4e14a-107">Quando si seleziona un file  *\_layout. cshtml* esistente, questo **non** viene sovrascritto.</span><span class="sxs-lookup"><span data-stu-id="4e14a-107">When an existing *\_Layout.cshtml* file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="4e14a-108">Ad esempio: `~/Pages/Shared/_Layout.cshtml` per Razor pages `~/Views/Shared/_Layout.cshtml` per i progetti MVC</span><span class="sxs-lookup"><span data-stu-id="4e14a-108">For example: `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
* <span data-ttu-id="4e14a-109">Per usare il contesto dei dati esistente, selezionare almeno un file di cui eseguire l'override.</span><span class="sxs-lookup"><span data-stu-id="4e14a-109">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="4e14a-110">È necessario selezionare almeno un file per aggiungere il contesto dei dati.</span><span class="sxs-lookup"><span data-stu-id="4e14a-110">You must select at least one file to add your data context.</span></span>
  * <span data-ttu-id="4e14a-111">Selezionare la classe del contesto dati.</span><span class="sxs-lookup"><span data-stu-id="4e14a-111">Select your data context class.</span></span>
  * <span data-ttu-id="4e14a-112">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="4e14a-112">Select **Add**.</span></span>
* <span data-ttu-id="4e14a-113">Per creare un nuovo contesto utente ed eventualmente creare una classe utente personalizzata per l'identità:</span><span class="sxs-lookup"><span data-stu-id="4e14a-113">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="4e14a-114">Selezionare il **+** per creare un nuovo pulsante **classe contesto dati**.</span><span class="sxs-lookup"><span data-stu-id="4e14a-114">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="4e14a-115">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="4e14a-115">Select **Add**.</span></span>

<span data-ttu-id="4e14a-116">Nota: Se si sta creando un nuovo contesto utente, non è necessario selezionare un file di cui eseguire l'override.</span><span class="sxs-lookup"><span data-stu-id="4e14a-116">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4e14a-117">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="4e14a-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="4e14a-118">Se l'utilità di scaffolding di ASP.NET Core non è stato precedentemente installato, installarlo ora:</span><span class="sxs-lookup"><span data-stu-id="4e14a-118">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```console
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="4e14a-119">Aggiungere un riferimento al pacchetto [Microsoft. VisualStudio. Web. CodeGeneration. Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) al file di\*progetto (con estensione csproj).</span><span class="sxs-lookup"><span data-stu-id="4e14a-119">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="4e14a-120">Eseguire il comando seguente nella directory del progetto:</span><span class="sxs-lookup"><span data-stu-id="4e14a-120">Run the following command in the project directory:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="4e14a-121">Eseguire il comando seguente per elencare le opzioni di utilità di scaffolding di identità:</span><span class="sxs-lookup"><span data-stu-id="4e14a-121">Run the following command to list the Identity scaffolder options:</span></span>

```console
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="4e14a-122">Nella cartella del progetto eseguire l'impalcatura di identità con le opzioni desiderate.</span><span class="sxs-lookup"><span data-stu-id="4e14a-122">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="4e14a-123">Ad esempio, per configurare Identity con l'interfaccia utente predefinita e il numero minimo di file, eseguire il comando seguente.</span><span class="sxs-lookup"><span data-stu-id="4e14a-123">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="4e14a-124">Usare il nome completo corretto per il contesto del database:</span><span class="sxs-lookup"><span data-stu-id="4e14a-124">Use the correct fully qualified name for your DB context:</span></span>

```console
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

<span data-ttu-id="4e14a-125">PowerShell usa il punto e virgola come separatore di comandi.</span><span class="sxs-lookup"><span data-stu-id="4e14a-125">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="4e14a-126">Quando si usa PowerShell, usare il carattere di escape per i punti e virgola nell'elenco dei file o inserire l'elenco di file tra virgolette doppie.</span><span class="sxs-lookup"><span data-stu-id="4e14a-126">When using PowerShell, escape the semi-colons in the file list or put the file list in double quotes.</span></span> <span data-ttu-id="4e14a-127">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="4e14a-127">For example:</span></span>

```console
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="4e14a-128">Se si esegue l'impalcatura delle identità senza specificare `--files` il flag o `--useDefaultUI` il flag, tutte le pagine dell'interfaccia utente di identità disponibili verranno create nel progetto.</span><span class="sxs-lookup"><span data-stu-id="4e14a-128">If you run the Identity scaffolder without specifying the `--files` flag or the `--useDefaultUI` flag, all the available Identity UI pages will be created in your project.</span></span>

---
