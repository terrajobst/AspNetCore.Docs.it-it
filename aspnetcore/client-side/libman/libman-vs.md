---
title: Usare LibMan con ASP.NET Core in Visual Studio
author: scottaddie
description: Informazioni su come usare LibMan in un progetto ASP.NET Core con Visual Studio.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/20/2018
uid: client-side/libman/libman-vs
ms.openlocfilehash: b44769f1d0925f38523d6570858de17f37e32c2b
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/22/2018
ms.locfileid: "41909988"
---
# <a name="use-libman-with-aspnet-core-in-visual-studio"></a><span data-ttu-id="cf2ef-103">Usare LibMan con ASP.NET Core in Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cf2ef-103">Use LibMan with ASP.NET Core in Visual Studio</span></span>

<span data-ttu-id="cf2ef-104">Di [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="cf2ef-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="cf2ef-105">Visual Studio offre supporto predefinito per [LibMan](xref:client-side/libman/index) nei progetti ASP.NET Core, tra cui:</span><span class="sxs-lookup"><span data-stu-id="cf2ef-105">Visual Studio has built-in support for [LibMan](xref:client-side/libman/index) in ASP.NET Core projects, including:</span></span>

* <span data-ttu-id="cf2ef-106">Supporto per la configurazione ed esecuzione di operazioni di ripristino LibMan in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-106">Support for configuring and running LibMan restore operations on build.</span></span>
* <span data-ttu-id="cf2ef-107">Voci di menu per l'attivazione LibMan ripristino e le operazioni di pulizia.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-107">Menu items for triggering LibMan restore and clean operations.</span></span>
* <span data-ttu-id="cf2ef-108">Finestra di dialogo di ricerca per trovare le librerie e aggiunge i file a un progetto.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-108">Search dialog for finding libraries and adding the files to a project.</span></span>
* <span data-ttu-id="cf2ef-109">Supporto per *libman.json*&mdash;LibMan file manifesto.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-109">Editing support for *libman.json*&mdash;the LibMan manifest file.</span></span>

<span data-ttu-id="cf2ef-110">[Visualizzare o scaricare codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/libman/samples/) [(come scaricare)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="cf2ef-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/libman/samples/) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cf2ef-111">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="cf2ef-111">Prerequisites</span></span>

* <span data-ttu-id="cf2ef-112">Visual Studio 2017 versione 15,8 o versione successiva con il **sviluppo ASP.NET e web** carico di lavoro</span><span class="sxs-lookup"><span data-stu-id="cf2ef-112">Visual Studio 2017 version 15.8 or later with the **ASP.NET and web development** workload</span></span>

## <a name="add-library-files"></a><span data-ttu-id="cf2ef-113">Aggiungere i file di libreria</span><span class="sxs-lookup"><span data-stu-id="cf2ef-113">Add library files</span></span>

<span data-ttu-id="cf2ef-114">I file di libreria possono essere aggiunti a un progetto ASP.NET Core in due modi diversi:</span><span class="sxs-lookup"><span data-stu-id="cf2ef-114">Library files can be added to an ASP.NET Core project in two different ways:</span></span>

1. [<span data-ttu-id="cf2ef-115">Utilizzare la finestra di dialogo Aggiungi libreria lato Client</span><span class="sxs-lookup"><span data-stu-id="cf2ef-115">Use the Add Client-Side Library dialog</span></span>](#use-the-add-client-side-library-dialog)
1. [<span data-ttu-id="cf2ef-116">Configurare manualmente le voci di file manifesto LibMan</span><span class="sxs-lookup"><span data-stu-id="cf2ef-116">Manually configure LibMan manifest file entries</span></span>](#manually-configure-libman-manifest-file-entries)

### <a name="use-the-add-client-side-library-dialog"></a><span data-ttu-id="cf2ef-117">Utilizzare la finestra di dialogo Aggiungi libreria lato Client</span><span class="sxs-lookup"><span data-stu-id="cf2ef-117">Use the Add Client-Side Library dialog</span></span>

<span data-ttu-id="cf2ef-118">Seguire questi passaggi per installare una libreria lato client:</span><span class="sxs-lookup"><span data-stu-id="cf2ef-118">Follow these steps to install a client-side library:</span></span>

* <span data-ttu-id="cf2ef-119">Nelle **Esplora soluzioni**, fare clic sulla cartella di progetto in cui i file devono essere aggiunti.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-119">In **Solution Explorer**, right-click the project folder in which the files should be added.</span></span> <span data-ttu-id="cf2ef-120">Scegli **aggiungere** > **libreria lato Client**.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-120">Choose **Add** > **Client-Side Library**.</span></span> <span data-ttu-id="cf2ef-121">Il **Aggiungi libreria lato Client** viene visualizzata la finestra:</span><span class="sxs-lookup"><span data-stu-id="cf2ef-121">The **Add Client-Side Library** dialog appears:</span></span>

  ![Aggiungi finestra di dialogo libreria lato Client](_static/add-library-dialog.png)

* <span data-ttu-id="cf2ef-123">Selezionare il provider della libreria dal **Provider** elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-123">Select the library provider from the **Provider** drop down.</span></span> <span data-ttu-id="cf2ef-124">CDNJS è il provider predefinito.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-124">CDNJS is the default provider.</span></span>
* <span data-ttu-id="cf2ef-125">Digitare il nome della libreria da recuperare nel **libreria** casella di testo.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-125">Type the library name to fetch in the **Library** text box.</span></span> <span data-ttu-id="cf2ef-126">IntelliSense offre un elenco di librerie che iniziano con il testo specificato.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-126">IntelliSense provides a list of libraries beginning with the provided text.</span></span>
* <span data-ttu-id="cf2ef-127">Selezionare la libreria dall'elenco di IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-127">Select the library from the IntelliSense list.</span></span> <span data-ttu-id="cf2ef-128">Si noti che il nome della libreria viene aggiunto il suffisso di `@` simboli e la versione stabile più recente noto per il provider selezionato.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-128">Notice the library name is suffixed with the `@` symbol and the latest stable version known to the selected provider.</span></span>
* <span data-ttu-id="cf2ef-129">Decidere quali file da includere:</span><span class="sxs-lookup"><span data-stu-id="cf2ef-129">Decide which files to include:</span></span>
  * <span data-ttu-id="cf2ef-130">Selezionare il **includere tutti i file di libreria** pulsante di opzione per includere tutti i file della libreria.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-130">Select the **Include all library files** radio button to include all of the library's files.</span></span>
  * <span data-ttu-id="cf2ef-131">Selezionare il **scegliere file specifici** pulsante di opzione per includere un sottoinsieme di file della libreria.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-131">Select the **Choose specific files** radio button to include a subset of the library's files.</span></span> <span data-ttu-id="cf2ef-132">Quando il pulsante di opzione è selezionato, l'albero di selettore file è abilitato.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-132">When the radio button is selected, the file selector tree is enabled.</span></span> <span data-ttu-id="cf2ef-133">Selezionare le caselle alla sinistra di nomi di file da scaricare.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-133">Check the boxes to the left of the file names to download.</span></span>
* <span data-ttu-id="cf2ef-134">Specificare la cartella di progetto per archiviare i file nei **percorso di destinazione** casella di testo.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-134">Specify the project folder for storing the files in the **Target Location** text box.</span></span> <span data-ttu-id="cf2ef-135">Sotto forma di suggerimento, archiviare tutte le librerie in una cartella separata.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-135">As a recommendation, store each library in a separate folder.</span></span>

  <span data-ttu-id="cf2ef-136">Suggeriti **percorso di destinazione** cartella è basata sulla posizione da cui avviare la finestra di dialogo:</span><span class="sxs-lookup"><span data-stu-id="cf2ef-136">The suggested **Target Location** folder is based on the location from which the dialog launched:</span></span>

  * <span data-ttu-id="cf2ef-137">Se avviato dalla radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="cf2ef-137">If launched from the project root:</span></span>
    * <span data-ttu-id="cf2ef-138">*Wwwroot/lib* viene usato se *wwwroot* esiste.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-138">*wwwroot/lib* is used if *wwwroot* exists.</span></span>
    * <span data-ttu-id="cf2ef-139">*lib* viene usato se *wwwroot* non esiste.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-139">*lib* is used if *wwwroot* doesn't exist.</span></span>
  * <span data-ttu-id="cf2ef-140">Se avviata da una cartella di progetto, viene utilizzato il nome della cartella corrispondente.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-140">If launched from a project folder, the corresponding folder name is used.</span></span>

  <span data-ttu-id="cf2ef-141">Il suggerimento di cartella viene aggiunto il suffisso con il nome della libreria.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-141">The folder suggestion is suffixed with the library name.</span></span> <span data-ttu-id="cf2ef-142">La tabella seguente illustra i suggerimenti di cartella quando si installa jQuery in un progetto Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-142">The following table illustrates folder suggestions when installing jQuery in a Razor Pages project.</span></span>
  
  |<span data-ttu-id="cf2ef-143">Posizione di avvio</span><span class="sxs-lookup"><span data-stu-id="cf2ef-143">Launch location</span></span>                           |<span data-ttu-id="cf2ef-144">Cartella suggerita</span><span class="sxs-lookup"><span data-stu-id="cf2ef-144">Suggested folder</span></span>      |
  |------------------------------------------|----------------------|
  |<span data-ttu-id="cf2ef-145">radice del progetto (se *wwwroot* esiste)</span><span class="sxs-lookup"><span data-stu-id="cf2ef-145">project root (if *wwwroot* exists)</span></span>        |<span data-ttu-id="cf2ef-146">*Wwwroot/lib/jquery /*</span><span class="sxs-lookup"><span data-stu-id="cf2ef-146">*wwwroot/lib/jquery/*</span></span> |
  |<span data-ttu-id="cf2ef-147">radice del progetto (se *wwwroot* non esiste)</span><span class="sxs-lookup"><span data-stu-id="cf2ef-147">project root (if *wwwroot* doesn't exist)</span></span> |<span data-ttu-id="cf2ef-148">*jquery/lib /*</span><span class="sxs-lookup"><span data-stu-id="cf2ef-148">*lib/jquery/*</span></span>         |
  |<span data-ttu-id="cf2ef-149">*Pagine* cartella nel progetto</span><span class="sxs-lookup"><span data-stu-id="cf2ef-149">*Pages* folder in project</span></span>                 |<span data-ttu-id="cf2ef-150">*Pagine/jquery /*</span><span class="sxs-lookup"><span data-stu-id="cf2ef-150">*Pages/jquery/*</span></span>       |

* <span data-ttu-id="cf2ef-151">Scegliere il **installare** pulsante per scaricare i file, per la configurazione nel *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-151">Click the **Install** button to download the files, per the configuration in *libman.json*.</span></span>
* <span data-ttu-id="cf2ef-152">Esaminare i **Gestione librerie** feed del **Output** finestra per i dettagli di installazione.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-152">Review the **Library Manager** feed of the **Output** window for installation details.</span></span> <span data-ttu-id="cf2ef-153">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cf2ef-153">For example:</span></span>

  ```console
  Restore operation started...
  Restoring libraries for project LibManSample
  Restoring library jquery@3.3.1... (LibManSample)
  wwwroot/lib/jquery/jquery.min.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.min.map written to destination (LibManSample)
  Restore operation completed
  1 libraries restored in 2.32 seconds
  ```

### <a name="manually-configure-libman-manifest-file-entries"></a><span data-ttu-id="cf2ef-154">Configurare manualmente le voci di file manifesto LibMan</span><span class="sxs-lookup"><span data-stu-id="cf2ef-154">Manually configure LibMan manifest file entries</span></span>

<span data-ttu-id="cf2ef-155">Tutte le operazioni di LibMan in Visual Studio sono basate sul contenuto del manifesto LibMan della radice del progetto (*libman.json*).</span><span class="sxs-lookup"><span data-stu-id="cf2ef-155">All LibMan operations in Visual Studio are based on the content of the project root's LibMan manifest (*libman.json*).</span></span> <span data-ttu-id="cf2ef-156">È possibile modificare manualmente *libman.json* per configurare i file di libreria per il progetto.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-156">You can manually edit *libman.json* to configure library files for the project.</span></span> <span data-ttu-id="cf2ef-157">Visual Studio Ripristina tutti i file di libreria, una volta *libman.json* viene salvato.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-157">Visual Studio restores all library files once *libman.json* is saved.</span></span>

<span data-ttu-id="cf2ef-158">Per aprire *libman.json* per la modifica, esistono le seguenti opzioni:</span><span class="sxs-lookup"><span data-stu-id="cf2ef-158">To open *libman.json* for editing, the following options exist:</span></span>

* <span data-ttu-id="cf2ef-159">Fare doppio clic il *libman.json* del file in **Esplora soluzioni**.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-159">Double-click the *libman.json* file in **Solution Explorer**.</span></span>
* <span data-ttu-id="cf2ef-160">Fare clic sul progetto in **Esplora soluzioni** e selezionare **Gestisci librerie lato Client**.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-160">Right-click the project in **Solution Explorer** and select **Manage Client-Side Libraries**.</span></span> <span data-ttu-id="cf2ef-161">**&#8224;**</span><span class="sxs-lookup"><span data-stu-id="cf2ef-161">**&#8224;**</span></span>
* <span data-ttu-id="cf2ef-162">Selezionare **gestire le librerie Client-Side** Visual Studio **progetto** menu.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-162">Select **Manage Client-Side Libraries** from the Visual Studio **Project** menu.</span></span> <span data-ttu-id="cf2ef-163">**&#8224;**</span><span class="sxs-lookup"><span data-stu-id="cf2ef-163">**&#8224;**</span></span>

<span data-ttu-id="cf2ef-164">**&#8224;** Se il *libman.json* file non esiste già nella radice del progetto, verrà creato con il contenuto del modello di elemento predefinito.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-164">**&#8224;** If the *libman.json* file doesn't already exist in the project root, it will be created with the default item template content.</span></span>

<span data-ttu-id="cf2ef-165">Visual Studio offre JSON completo, supporto, ad esempio la colorazione di modifica, formattazione, IntelliSense e convalida dello schema.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-165">Visual Studio offers rich JSON editing support such as colorization, formatting, IntelliSense, and schema validation.</span></span> <span data-ttu-id="cf2ef-166">Schema JSON del manifesto LibMan si trova in corrispondenza [ http://json.schemastore.org/libman ](http://json.schemastore.org/libman).</span><span class="sxs-lookup"><span data-stu-id="cf2ef-166">The LibMan manifest's JSON schema is found at [http://json.schemastore.org/libman](http://json.schemastore.org/libman).</span></span>

<span data-ttu-id="cf2ef-167">Con il file di manifesto seguente, LibMan recupera i file per la configurazione definita nel `libraries` proprietà.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-167">With the following manifest file, LibMan retrieves files per the configuration defined in the `libraries` property.</span></span> <span data-ttu-id="cf2ef-168">Una spiegazione dei valori letterali di oggetto definiti all'interno di `libraries` segue:</span><span class="sxs-lookup"><span data-stu-id="cf2ef-168">An explanation of the object literals defined within `libraries` follows:</span></span>

* <span data-ttu-id="cf2ef-169">Un subset delle [jQuery](https://jquery.com/) versione 3.3.1 viene recuperato dal provider CDNJS.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-169">A subset of [jQuery](https://jquery.com/) version 3.3.1 is retrieved from the CDNJS provider.</span></span> <span data-ttu-id="cf2ef-170">Il subset è definito nel `files` proprietà&mdash;*jquery.min.js*, *jquery.js*, e *jquery.min.map*.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-170">The subset is defined in the `files` property&mdash;*jquery.min.js*, *jquery.js*, and *jquery.min.map*.</span></span> <span data-ttu-id="cf2ef-171">I file vengono inseriti del progetto *wwwroot/lib/jquery* cartella.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-171">The files are placed in the project's *wwwroot/lib/jquery* folder.</span></span>
* <span data-ttu-id="cf2ef-172">Nella sua totalità [Bootstrap](https://getbootstrap.com/) versione 4.1.3 viene recuperato e inserita in un *wwwroot/lib/bootstrap* cartella.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-172">The entirety of [Bootstrap](https://getbootstrap.com/) version 4.1.3 is retrieved and placed in a *wwwroot/lib/bootstrap* folder.</span></span> <span data-ttu-id="cf2ef-173">Del valore letterale di oggetto `provider` override delle proprietà di `defaultProvider` valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-173">The object literal's `provider` property overrides the `defaultProvider` property value.</span></span> <span data-ttu-id="cf2ef-174">LibMan recupera i file Bootstrap del provider unpkg.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-174">LibMan retrieves the Bootstrap files from the unpkg provider.</span></span>
* <span data-ttu-id="cf2ef-175">Un subset delle [Lodash](https://lodash.com/) è stata approvata da un corpo che implementano la governance all'interno dell'organizzazione.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-175">A subset of [Lodash](https://lodash.com/) was approved by a governing body within the organization.</span></span> <span data-ttu-id="cf2ef-176">Il *lodash.js* e *lodash.min.js* i file vengono recuperati dal file system locale a *c:\\tmp\\*.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-176">The *lodash.js* and *lodash.min.js* files are retrieved from the local file system at *C:\\tmp\\*.</span></span> <span data-ttu-id="cf2ef-177">I file vengono copiati nel progetto *wwwroot/lib/lodash* cartella.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-177">The files are copied to the project's *wwwroot/lib/lodash* folder.</span></span>

[!code-json[](samples/LibManSample/libman.json)]

> [!NOTE]
> <span data-ttu-id="cf2ef-178">LibMan supporta solo una versione di tutte le librerie da ogni provider.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-178">LibMan only supports one version of each library from each provider.</span></span> <span data-ttu-id="cf2ef-179">Il *libman.json* file ha esito negativo di convalida dello schema, se contiene due raccolte con lo stesso nome di libreria per un determinato provider.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-179">The *libman.json* file fails schema validation if it contains two libraries with the same library name for a given provider.</span></span>

## <a name="restore-library-files"></a><span data-ttu-id="cf2ef-180">Ripristinare i file di libreria</span><span class="sxs-lookup"><span data-stu-id="cf2ef-180">Restore library files</span></span>

<span data-ttu-id="cf2ef-181">Per ripristinare i file di libreria dall'interno di Visual Studio, è necessario un valore valido *libman.json* file nella radice del progetto.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-181">To restore library files from within Visual Studio, there must be a valid *libman.json* file in the project root.</span></span> <span data-ttu-id="cf2ef-182">File ripristinati vengono inseriti nel progetto in corrispondenza della posizione specificata per ogni raccolta.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-182">Restored files are placed in the project at the location specified for each library.</span></span>

<span data-ttu-id="cf2ef-183">I file di libreria possono essere ripristinati in un progetto ASP.NET Core in due modi:</span><span class="sxs-lookup"><span data-stu-id="cf2ef-183">Library files can be restored in an ASP.NET Core project in two ways:</span></span>

1. [<span data-ttu-id="cf2ef-184">Ripristinare i file durante la compilazione</span><span class="sxs-lookup"><span data-stu-id="cf2ef-184">Restore files during build</span></span>](#restore-files-during-build)
1. [<span data-ttu-id="cf2ef-185">Ripristinare i file manualmente</span><span class="sxs-lookup"><span data-stu-id="cf2ef-185">Restore files manually</span></span>](#restore-files-manually)

### <a name="restore-files-during-build"></a><span data-ttu-id="cf2ef-186">Ripristinare i file durante la compilazione</span><span class="sxs-lookup"><span data-stu-id="cf2ef-186">Restore files during build</span></span>

<span data-ttu-id="cf2ef-187">LibMan puoi ripristinare i file di libreria definiti come parte del processo di compilazione.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-187">LibMan can restore the defined library files as part of the build process.</span></span> <span data-ttu-id="cf2ef-188">Per impostazione predefinita, il *ripristino in fase di compilazione* comportamento è disabilitato.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-188">By default, the *restore-on-build* behavior is disabled.</span></span>

<span data-ttu-id="cf2ef-189">Per abilitare e testare il comportamento di ripristino in fase di compilazione:</span><span class="sxs-lookup"><span data-stu-id="cf2ef-189">To enable and test the restore-on-build behavior:</span></span>

* <span data-ttu-id="cf2ef-190">Fare doppio clic su *libman.json* nelle **Esplora soluzioni** e selezionare **abilitare ripristino configurazione di librerie lato Client in fase di compilazione** dal menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-190">Right-click *libman.json* in **Solution Explorer** and select **Enable Restore Client-Side Libraries on Build** from the context menu.</span></span>
* <span data-ttu-id="cf2ef-191">Scegliere il **Sì** pulsante quando viene richiesto di installare un pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-191">Click the **Yes** button when prompted to install a NuGet package.</span></span> <span data-ttu-id="cf2ef-192">Il [Microsoft.Web.LibraryManager.Build](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Build/) pacchetto NuGet viene aggiunto al progetto:</span><span class="sxs-lookup"><span data-stu-id="cf2ef-192">The [Microsoft.Web.LibraryManager.Build](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Build/) NuGet package is added to the project:</span></span>

  [!code-xml[](samples/LibManSample/LibManSample.csproj?name=snippet_RestoreOnBuildPackage)]

* <span data-ttu-id="cf2ef-193">Compilare il progetto per verificare che il ripristino dei file LibMan si verifica.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-193">Build the project to confirm LibMan file restoration occurs.</span></span> <span data-ttu-id="cf2ef-194">Il `Microsoft.Web.LibraryManager.Build` pacchetto inserisce una destinazione di MSBuild che esegue LibMan durante l'operazione di compilazione del progetto.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-194">The `Microsoft.Web.LibraryManager.Build` package injects an MSBuild target that runs LibMan during the project's build operation.</span></span>
* <span data-ttu-id="cf2ef-195">Esaminare la **compilare** feed del **Output** finestra per il registro attività LibMan:</span><span class="sxs-lookup"><span data-stu-id="cf2ef-195">Review the **Build** feed of the **Output** window for a LibMan activity log:</span></span>

  ```console
  1>------ Build started: Project: LibManSample, Configuration: Debug Any CPU ------
  1>
  1>Restore operation started...
  1>Restoring library jquery@3.3.1...
  1>Restoring library bootstrap@4.1.3...
  1>
  1>2 libraries restored in 10.66 seconds
  1>LibManSample -> C:\LibManSample\bin\Debug\netcoreapp2.1\LibManSample.dll
  ========== Build: 1 succeeded, 0 failed, 0 up-to-date, 0 skipped ==========
  ```

<span data-ttu-id="cf2ef-196">Quando è abilitato il comportamento di ripristino in fase di compilazione, il *libman.json* menu di scelta rapida visualizza un **disabilitare ripristino configurazione di librerie lato Client in fase di compilazione** opzione.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-196">When the restore-on-build behavior is enabled, the *libman.json* context menu displays a **Disable Restore Client-Side Libraries on Build** option.</span></span> <span data-ttu-id="cf2ef-197">Se si seleziona questa opzione rimuove il `Microsoft.Web.LibraryManager.Build` riferimento dal file di progetto del pacchetto.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-197">Selecting this option removes the `Microsoft.Web.LibraryManager.Build` package reference from the project file.</span></span> <span data-ttu-id="cf2ef-198">Di conseguenza, le librerie lato client non vengono più ripristinate in ogni fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-198">Consequently, the client-side libraries are no longer restored on each build.</span></span>

<span data-ttu-id="cf2ef-199">Indipendentemente dall'impostazione di ripristino in fase di compilazione, è possibile ripristinare manualmente in qualsiasi momento dal *libman.json* menu di scelta rapida.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-199">Regardless of the restore-on-build setting, you can manually restore at any time from the *libman.json* context menu.</span></span> <span data-ttu-id="cf2ef-200">Per altre informazioni, vedere [ripristinare manualmente i file](#restore-files-manually).</span><span class="sxs-lookup"><span data-stu-id="cf2ef-200">For more information, see [Restore files manually](#restore-files-manually).</span></span>

### <a name="restore-files-manually"></a><span data-ttu-id="cf2ef-201">Ripristinare i file manualmente</span><span class="sxs-lookup"><span data-stu-id="cf2ef-201">Restore files manually</span></span>

<span data-ttu-id="cf2ef-202">Per ripristinare manualmente i file di libreria:</span><span class="sxs-lookup"><span data-stu-id="cf2ef-202">To manually restore library files:</span></span>

* <span data-ttu-id="cf2ef-203">Per tutti i progetti nella soluzione:</span><span class="sxs-lookup"><span data-stu-id="cf2ef-203">For all projects in the solution:</span></span>
  * <span data-ttu-id="cf2ef-204">Fare clic sul nome della soluzione in **Esplora soluzioni**.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-204">Right-click the solution name in **Solution Explorer**.</span></span>
  * <span data-ttu-id="cf2ef-205">Selezionare il **ripristinare librerie lato Client** opzione.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-205">Select the **Restore Client-Side Libraries** option.</span></span>
* <span data-ttu-id="cf2ef-206">Per un progetto specifico:</span><span class="sxs-lookup"><span data-stu-id="cf2ef-206">For a specific project:</span></span>
  * <span data-ttu-id="cf2ef-207">Fare doppio clic il *libman.json* del file in **Esplora soluzioni**.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-207">Right-click the *libman.json* file in **Solution Explorer**.</span></span>
  * <span data-ttu-id="cf2ef-208">Selezionare il **ripristinare librerie lato Client** opzione.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-208">Select the **Restore Client-Side Libraries** option.</span></span>

<span data-ttu-id="cf2ef-209">Durante l'esecuzione dell'operazione di ripristino:</span><span class="sxs-lookup"><span data-stu-id="cf2ef-209">While the restore operation is running:</span></span>

* <span data-ttu-id="cf2ef-210">Icona del centro stato attività (TSC) sulla barra di stato di Visual Studio verrà animato e leggerà *avviato l'operazione di ripristino*.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-210">The Task Status Center (TSC) icon on the Visual Studio status bar will be animated and will read *Restore operation started*.</span></span> <span data-ttu-id="cf2ef-211">Facendo clic sull'icona viene visualizzata la descrizione comando Elenca le attività in background noti.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-211">Clicking the icon opens a tooltip listing the known background tasks.</span></span>
* <span data-ttu-id="cf2ef-212">Messaggi verranno inviati nella barra di stato e il **Gestione librerie** feed del **Output** finestra.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-212">Messages will be sent to the status bar and the **Library Manager** feed of the **Output** window.</span></span> <span data-ttu-id="cf2ef-213">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cf2ef-213">For example:</span></span>

  ```console
  Restore operation started...
  Restoring libraries for project LibManSample
  Restoring library jquery@3.3.1... (LibManSample)
  wwwroot/lib/jquery/jquery.min.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.js written to destination (LibManSample)
  wwwroot/lib/jquery/jquery.min.map written to destination (LibManSample)
  Restore operation completed
  1 libraries restored in 2.32 seconds
  ```

## <a name="delete-library-files"></a><span data-ttu-id="cf2ef-214">Eliminare i file di libreria</span><span class="sxs-lookup"><span data-stu-id="cf2ef-214">Delete library files</span></span>

<span data-ttu-id="cf2ef-215">Per eseguire la *pulita* , operazione che comporta l'eliminazione di file di libreria precedentemente ripristinati in Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="cf2ef-215">To perform the *clean* operation, which deletes library files previously restored in Visual Studio:</span></span>

* <span data-ttu-id="cf2ef-216">Fare doppio clic il *libman.json* del file in **Esplora soluzioni**.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-216">Right-click the *libman.json* file in **Solution Explorer**.</span></span>
* <span data-ttu-id="cf2ef-217">Selezionare il **pulita librerie lato Client** opzione.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-217">Select the **Clean Client-Side Libraries** option.</span></span>

<span data-ttu-id="cf2ef-218">Per evitare la rimozione accidentale di file non di libreria, l'operazione di pulizia non comporta l'eliminazione intere directory.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-218">To prevent unintentional removal of non-library files, the clean operation doesn't delete whole directories.</span></span> <span data-ttu-id="cf2ef-219">Rimuove solo i file che sono stati inclusi nel ripristino precedente.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-219">It only removes files that were included in the previous restore.</span></span>

<span data-ttu-id="cf2ef-220">Mentre l'operazione di pulizia è in esecuzione:</span><span class="sxs-lookup"><span data-stu-id="cf2ef-220">While the clean operation is running:</span></span>

* <span data-ttu-id="cf2ef-221">L'icona TSC sulla barra di stato di Visual Studio verrà animato e leggerà *operazione le librerie Client avviata*.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-221">The TSC icon on the Visual Studio status bar will be animated and will read *Client libraries operation started*.</span></span> <span data-ttu-id="cf2ef-222">Facendo clic sull'icona viene visualizzata la descrizione comando Elenca le attività in background noti.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-222">Clicking the icon opens a tooltip listing the known background tasks.</span></span>
* <span data-ttu-id="cf2ef-223">I messaggi vengono inviati nella barra di stato e il **Gestione librerie** feed del **Output** finestra.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-223">Messages are sent to the status bar and the **Library Manager** feed of the **Output** window.</span></span> <span data-ttu-id="cf2ef-224">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="cf2ef-224">For example:</span></span>

```console
Clean libraries operation started...
Clean libraries operation completed
2 libraries were successfully deleted in 1.91 secs
```

<span data-ttu-id="cf2ef-225">L'operazione di pulizia Elimina solo i file dal progetto.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-225">The clean operation only deletes files from the project.</span></span> <span data-ttu-id="cf2ef-226">File della libreria di rimangono nella cache per il recupero più veloce sulle operazioni di ripristino future.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-226">Library files stay in the cache for faster retrieval on future restore operations.</span></span> <span data-ttu-id="cf2ef-227">Per gestire i file di libreria memorizzati nella cache del computer locale, usare la CLI di LibMan.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-227">To manage library files stored in the local machine's cache, use the LibMan CLI.</span></span>

## <a name="uninstall-library-files"></a><span data-ttu-id="cf2ef-228">Disinstallare i file di libreria</span><span class="sxs-lookup"><span data-stu-id="cf2ef-228">Uninstall library files</span></span>

<span data-ttu-id="cf2ef-229">Per disinstallare i file di libreria:</span><span class="sxs-lookup"><span data-stu-id="cf2ef-229">To uninstall library files:</span></span>

* <span data-ttu-id="cf2ef-230">Aprire *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-230">Open *libman.json*.</span></span>
* <span data-ttu-id="cf2ef-231">Posizionare il cursore all'interno di corrispondente `libraries` valore letterale di oggetto.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-231">Position the caret inside the corresponding `libraries` object literal.</span></span>
* <span data-ttu-id="cf2ef-232">Scegliere l'icona lampadina visualizzata nel margine sinistro e selezionare **disinstallazione \<library_name > @\<library_version >**:</span><span class="sxs-lookup"><span data-stu-id="cf2ef-232">Click the light bulb icon that appears in the left margin, and select **Uninstall \<library_name>@\<library_version>**:</span></span>

  ![Disinstallare l'opzione del menu di scelta rapida della libreria](_static/uninstall-menu-option.png)

<span data-ttu-id="cf2ef-234">In alternativa, è possibile manualmente modificare e salvare il manifesto LibMan (*libman.json*).</span><span class="sxs-lookup"><span data-stu-id="cf2ef-234">Alternatively, you can manually edit and save the LibMan manifest (*libman.json*).</span></span> <span data-ttu-id="cf2ef-235">Il [l'operazione di ripristino](#restore-library-files) viene eseguito quando viene salvato il file.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-235">The [restore operation](#restore-library-files) runs when the file is saved.</span></span> <span data-ttu-id="cf2ef-236">File di libreria non è più definiti nella *libman.json* vengono rimossi dal progetto.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-236">Library files that are no longer defined in *libman.json* are removed from the project.</span></span>

## <a name="update-library-version"></a><span data-ttu-id="cf2ef-237">Versione di aggiornamento libreria</span><span class="sxs-lookup"><span data-stu-id="cf2ef-237">Update library version</span></span>

<span data-ttu-id="cf2ef-238">Per controllare una versione di aggiornamento della libreria:</span><span class="sxs-lookup"><span data-stu-id="cf2ef-238">To check for an updated library version:</span></span>

* <span data-ttu-id="cf2ef-239">Aprire *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-239">Open *libman.json*.</span></span>
* <span data-ttu-id="cf2ef-240">Posizionare il cursore all'interno di corrispondente `libraries` valore letterale di oggetto.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-240">Position the caret inside the corresponding `libraries` object literal.</span></span>
* <span data-ttu-id="cf2ef-241">Scegliere l'icona lampadina visualizzata nel margine sinistro.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-241">Click the light bulb icon that appears in the left margin.</span></span> <span data-ttu-id="cf2ef-242">Passare il mouse su **verificare la presenza di aggiornamenti**.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-242">Hover over **Check for updates**.</span></span>

<span data-ttu-id="cf2ef-243">LibMan controlla una versione della libreria più recente della versione installata.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-243">LibMan checks for a library version newer than the version installed.</span></span> <span data-ttu-id="cf2ef-244">Possono verificarsi i seguenti risultati:</span><span class="sxs-lookup"><span data-stu-id="cf2ef-244">The following outcomes can occur:</span></span>

* <span data-ttu-id="cf2ef-245">Oggetto **non sono stati trovati aggiornamenti** messaggio viene visualizzato se è già installata la versione più recente.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-245">A **No updates found** message is displayed if the latest version is already installed.</span></span>
* <span data-ttu-id="cf2ef-246">La versione stabile più recente viene visualizzata se non già installata.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-246">The latest stable version is displayed if not already installed.</span></span>

  ![Verificare la presenza di opzione del menu di scelta rapida degli aggiornamenti](_static/update-menu-option.png)

* <span data-ttu-id="cf2ef-248">Se è disponibile una versione non definitiva più recente della versione installata, viene visualizzata la versione non definitiva.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-248">If a pre-release newer than the installed version is available, the pre-release is displayed.</span></span>

<span data-ttu-id="cf2ef-249">Per effettuare il downgrade a una versione precedente di libreria, modificare manualmente il *libman.json* file.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-249">To downgrade to an older library version, manually edit the *libman.json* file.</span></span> <span data-ttu-id="cf2ef-250">Quando il file viene salvato, la LibMan [l'operazione di ripristino](#restore-library-files):</span><span class="sxs-lookup"><span data-stu-id="cf2ef-250">When the file is saved, the LibMan [restore operation](#restore-library-files):</span></span>

* <span data-ttu-id="cf2ef-251">Rimuove i file con ridondanza della versione precedente.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-251">Removes redundant files from the previous version.</span></span>
* <span data-ttu-id="cf2ef-252">Aggiunge i file nuovi e aggiornati della nuova versione.</span><span class="sxs-lookup"><span data-stu-id="cf2ef-252">Adds new and updated files from the new version.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cf2ef-253">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="cf2ef-253">Additional resources</span></span>

* [<span data-ttu-id="cf2ef-254">Repository LibMan GitHub</span><span class="sxs-lookup"><span data-stu-id="cf2ef-254">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
