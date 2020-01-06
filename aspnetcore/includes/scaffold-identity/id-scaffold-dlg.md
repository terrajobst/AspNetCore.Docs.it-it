<span data-ttu-id="fcf91-101">Eseguire l'impalcatura delle identità:</span><span class="sxs-lookup"><span data-stu-id="fcf91-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fcf91-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fcf91-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="fcf91-103">Dal **Esplora soluzioni**, fare doppio clic sul progetto > **Add** > **nuovo elemento di scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="fcf91-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="fcf91-104">Dal riquadro sinistro della finestra di **Add Scaffold** finestra di dialogo, seleziona **identità** > **aggiungere**.</span><span class="sxs-lookup"><span data-stu-id="fcf91-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="fcf91-105">Nella finestra di dialogo **Aggiungi identità** selezionare le opzioni desiderate.</span><span class="sxs-lookup"><span data-stu-id="fcf91-105">In the **ADD Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="fcf91-106">Selezionare la pagina layout esistente. in alternativa, il file di layout verrà sovrascritto con un markup errato.</span><span class="sxs-lookup"><span data-stu-id="fcf91-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="fcf91-107">Ad esempio `~/Pages/Shared/_Layout.cshtml` per Razor Pages `~/Views/Shared/_Layout.cshtml` per i progetti MVC</span><span class="sxs-lookup"><span data-stu-id="fcf91-107">For example `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
  * <span data-ttu-id="fcf91-108">Selezionare il **+** per creare un nuovo pulsante **classe contesto dati**.</span><span class="sxs-lookup"><span data-stu-id="fcf91-108">Select the **+** button to create a new **Data context class**.</span></span>
* <span data-ttu-id="fcf91-109">Selezionare **aggiungere**.</span><span class="sxs-lookup"><span data-stu-id="fcf91-109">Select **ADD**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="fcf91-110">Interfaccia della riga di comando di .NET Core</span><span class="sxs-lookup"><span data-stu-id="fcf91-110">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="fcf91-111">Se l'utilità di scaffolding di ASP.NET Core non è stato precedentemente installato, installarlo ora:</span><span class="sxs-lookup"><span data-stu-id="fcf91-111">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="fcf91-112">Aggiungere i riferimenti ai pacchetti NuGet necessari al file del progetto (\*. csproj).</span><span class="sxs-lookup"><span data-stu-id="fcf91-112">Add required NuGet package references to the project (\*.csproj) file.</span></span> <span data-ttu-id="fcf91-113">Eseguire il comando seguente nella directory del progetto:</span><span class="sxs-lookup"><span data-stu-id="fcf91-113">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="fcf91-114">Eseguire il comando seguente per elencare le opzioni di utilità di scaffolding di identità:</span><span class="sxs-lookup"><span data-stu-id="fcf91-114">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

[!INCLUDE[](~/includes/scaffoldTFM.md)]

<span data-ttu-id="fcf91-115">Nella cartella del progetto eseguire l'impalcatura di identità con le opzioni desiderate.</span><span class="sxs-lookup"><span data-stu-id="fcf91-115">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="fcf91-116">Ad esempio, per configurare Identity con l'interfaccia utente predefinita e il numero minimo di file, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="fcf91-116">For example, to setup identity with the default UI and the minimum number of files, run the following command:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity --useDefaultUI
```

---
