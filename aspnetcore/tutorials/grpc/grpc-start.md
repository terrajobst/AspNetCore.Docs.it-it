---
title: Creare un client e un server gRPC .NET Core in ASP.NET Core
author: juntaoluo
description: Questa esercitazione illustra come creare un servizio gRPC e un client gRPC in ASP.NET Core. Informazioni su come creare un progetto di servizio gRPC, modificare un file con estensione proto e aggiungere una chiamata con flusso bidirezionale.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 5/30/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 2b4325d2413e335a3061a7695def88a1b23ee52b
ms.sourcegitcommit: 4d05e30567279072f1b070618afe58ae1bcefd5a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376367"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="2c78e-104">Esercitazione: Creare un client e un server gRPC in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2c78e-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="2c78e-105">A cura di [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="2c78e-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="2c78e-106">Questa esercitazione illustra come creare un client [gRPC](https://grpc.io/docs/guides/) .NET Core e un server gRPC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2c78e-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="2c78e-107">Al termine, si otterrà un client gRPC che comunica con il servizio Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="2c78e-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="2c78e-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="2c78e-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="2c78e-109">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="2c78e-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2c78e-110">Creare un server gRPC.</span><span class="sxs-lookup"><span data-stu-id="2c78e-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="2c78e-111">Creare un client gRPC.</span><span class="sxs-lookup"><span data-stu-id="2c78e-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="2c78e-112">Testare il servizio client gRPC con il servizio Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="2c78e-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a><span data-ttu-id="2c78e-113">Creare un servizio gRPC</span><span class="sxs-lookup"><span data-stu-id="2c78e-113">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2c78e-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2c78e-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2c78e-115">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="2c78e-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="2c78e-116">Nella finestra di dialogo **Crea un nuovo progetto** selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="2c78e-116">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="2c78e-117">Scegliere **Avanti**</span><span class="sxs-lookup"><span data-stu-id="2c78e-117">Select **Next**</span></span>
* <span data-ttu-id="2c78e-118">Denominare il progetto **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="2c78e-118">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="2c78e-119">È importante denominare il progetto *GrpcGreeter* in modo che gli spazi dei nomi corrispondano nell'operazione copia/incolla del codice.</span><span class="sxs-lookup"><span data-stu-id="2c78e-119">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="2c78e-120">Selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="2c78e-120">Select **Create**</span></span>
* <span data-ttu-id="2c78e-121">Nella finestra di dialogo **Crea una nuova applicazione Web ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="2c78e-121">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="2c78e-122">Selezionare **.NET Core** e **ASP.NET Core 3.0** negli elenchi a discesa.</span><span class="sxs-lookup"><span data-stu-id="2c78e-122">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="2c78e-123">Selezionare il modello del **servizio gRPC**.</span><span class="sxs-lookup"><span data-stu-id="2c78e-123">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="2c78e-124">Selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="2c78e-124">Select **Create**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2c78e-125">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2c78e-125">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="2c78e-126">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="2c78e-126">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="2c78e-127">Cambiare directory (`cd`) e passare alla cartella che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="2c78e-127">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="2c78e-128">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="2c78e-128">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="2c78e-129">Il comando `dotnet new` crea un nuovo servizio gRPC nella cartella *GrpcGreeter*.</span><span class="sxs-lookup"><span data-stu-id="2c78e-129">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="2c78e-130">Il comando `code` apre la cartella *GrpcGreeter* in una nuova istanza di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="2c78e-130">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="2c78e-131">Viene visualizzata una finestra di dialogo con **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?** (Risorse di compilazione e debug mancanti da "RazorPagesMovie". Aggiungerle?)</span><span class="sxs-lookup"><span data-stu-id="2c78e-131">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="2c78e-132">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="2c78e-132">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2c78e-133">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="2c78e-133">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="2c78e-134">Da un terminale eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="2c78e-134">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="2c78e-135">I comandi precedenti usano l'[interfaccia della riga di comando di .NET Core](/dotnet/core/tools/dotnet) per creare un servizio gRPC.</span><span class="sxs-lookup"><span data-stu-id="2c78e-135">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="2c78e-136">Aprire il progetto</span><span class="sxs-lookup"><span data-stu-id="2c78e-136">Open the project</span></span>

<span data-ttu-id="2c78e-137">In Visual Studio selezionare **File > Apri** e quindi selezionare il file *GrpcGreeter.sln*.</span><span class="sxs-lookup"><span data-stu-id="2c78e-137">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="2c78e-138">Eseguire il servizio</span><span class="sxs-lookup"><span data-stu-id="2c78e-138">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2c78e-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2c78e-139">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2c78e-140">Premere CTRL+F5 per eseguire il servizio gRPC senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="2c78e-140">Press Ctrl+F5 to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="2c78e-141">Visual Studio esegue il servizio in un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="2c78e-141">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="2c78e-142">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="2c78e-142">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="2c78e-143">Eseguire il progetto gRPC Greeter GrpcGreeter dalla riga di comando tramite `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="2c78e-143">Run the gRPC Greeter project GrpcGreeter from the command line using `dotnet run`.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="2c78e-144">I log indicano che il servizio è in ascolto su `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="2c78e-144">The logs show the service listening on `http://localhost:50051`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

### <a name="examine-the-project-files"></a><span data-ttu-id="2c78e-145">Esaminare i file di progetto</span><span class="sxs-lookup"><span data-stu-id="2c78e-145">Examine the project files</span></span>

<span data-ttu-id="2c78e-146">File GrpcGreeter:</span><span class="sxs-lookup"><span data-stu-id="2c78e-146">GrpcGreeter files:</span></span>

* <span data-ttu-id="2c78e-147">*greet.proto*: Il file *Protos/greet.proto* definisce il gRPC `Greeter` e viene usato per generare le risorse del server gRPC.</span><span class="sxs-lookup"><span data-stu-id="2c78e-147">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="2c78e-148">Per altre informazioni, vedere [Introduzione a gRPC](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="2c78e-148">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="2c78e-149">Cartella *Servizi*: contiene l'implementazione del servizio `Greeter`.</span><span class="sxs-lookup"><span data-stu-id="2c78e-149">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="2c78e-150">*appSettings.json*: contiene i dati di configurazione, ad esempio il protocollo usato da Kestrel.</span><span class="sxs-lookup"><span data-stu-id="2c78e-150">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="2c78e-151">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="2c78e-151">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="2c78e-152">*Program.cs*: Contiene il punto di ingresso per il servizio gRPC.</span><span class="sxs-lookup"><span data-stu-id="2c78e-152">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="2c78e-153">Per ulteriori informazioni, vedere <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="2c78e-153">For more information, see <xref:fundamentals/host/web-host>.</span></span>
* <span data-ttu-id="2c78e-154">*Startup.cs*: Contiene codice che consente di configurare il comportamento dell'app.</span><span class="sxs-lookup"><span data-stu-id="2c78e-154">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="2c78e-155">Per altre informazioni, vedere [Avvio dell'app](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="2c78e-155">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="2c78e-156">Creare il client gRPC in un'app console .NET</span><span class="sxs-lookup"><span data-stu-id="2c78e-156">Create the gRPC client in a .NET console app</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2c78e-157">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2c78e-157">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2c78e-158">Selezionare **File** > **Nuovo** > **Progetto** dalla barra dei menu.</span><span class="sxs-lookup"><span data-stu-id="2c78e-158">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="2c78e-159">Nella finestra di dialogo **Crea un nuovo progetto** selezionare **App console (.NET Core)** .</span><span class="sxs-lookup"><span data-stu-id="2c78e-159">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="2c78e-160">Scegliere **Avanti**</span><span class="sxs-lookup"><span data-stu-id="2c78e-160">Select **Next**</span></span>
* <span data-ttu-id="2c78e-161">Nella casella di testo **Nome** immettere "GrpcGreeterClient".</span><span class="sxs-lookup"><span data-stu-id="2c78e-161">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="2c78e-162">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="2c78e-162">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2c78e-163">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2c78e-163">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="2c78e-164">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="2c78e-164">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="2c78e-165">Cambiare directory (`cd`) e passare alla cartella che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="2c78e-165">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="2c78e-166">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="2c78e-166">Run the following commands:</span></span>

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2c78e-167">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="2c78e-167">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="2c78e-168">Seguire le istruzioni riportate [qui](/dotnet/core/tutorials/using-on-mac-vs-full-solution) per creare un'app console con il nome *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="2c78e-168">Follow the instructions [here](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a><span data-ttu-id="2c78e-169">Aggiungere i pacchetti necessari</span><span class="sxs-lookup"><span data-stu-id="2c78e-169">Add required packages</span></span>

<span data-ttu-id="2c78e-170">Aggiungere i pacchetti seguenti nel progetto client gRPC:</span><span class="sxs-lookup"><span data-stu-id="2c78e-170">Add the following packages to the gRPC client project:</span></span>

* <span data-ttu-id="2c78e-171">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core), che contiene l'API C# per il client C-core.</span><span class="sxs-lookup"><span data-stu-id="2c78e-171">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core), which contains the C# API for the C-core client.</span></span>
* <span data-ttu-id="2c78e-172">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), che contiene le API per i messaggi protobuf per C#.</span><span class="sxs-lookup"><span data-stu-id="2c78e-172">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="2c78e-173">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), che contiene il supporto di strumenti C# per i file protobuf.</span><span class="sxs-lookup"><span data-stu-id="2c78e-173">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="2c78e-174">Il pacchetto di strumenti non è necessario in fase di esecuzione, quindi la dipendenza è contrassegnata come `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="2c78e-174">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2c78e-175">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2c78e-175">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2c78e-176">Installare i pacchetti tramite la Console di gestione pacchetti (PMC) o Gestisci pacchetti NuGet</span><span class="sxs-lookup"><span data-stu-id="2c78e-176">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Package</span></span>

####  <a name="pmc-option-to-install-packages"></a><span data-ttu-id="2c78e-177">Opzione con la console di Gestione pacchetti per installare i pacchetti</span><span class="sxs-lookup"><span data-stu-id="2c78e-177">PMC option to install packages</span></span>

* <span data-ttu-id="2c78e-178">Da Visual Studio selezionare **Strumenti** > **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="2c78e-178">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="2c78e-179">Dalla finestra **Console di Gestione pacchetti** passare alla directory in cui si trova il file *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="2c78e-179">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="2c78e-180">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="2c78e-180">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Core
Install-Package Grpc.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="2c78e-181">Opzione Gestisci pacchetti NuGet per installare i pacchetti</span><span class="sxs-lookup"><span data-stu-id="2c78e-181">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="2c78e-182">Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** > **Gestisci pacchetti NuGet**</span><span class="sxs-lookup"><span data-stu-id="2c78e-182">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="2c78e-183">Selezionare la scheda **Sfoglia**.</span><span class="sxs-lookup"><span data-stu-id="2c78e-183">Select the **Browse** tab.</span></span>
* <span data-ttu-id="2c78e-184">Immettere **Grpc.Core** nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="2c78e-184">Enter **Grpc.Core** in the search box.</span></span>
* <span data-ttu-id="2c78e-185">Selezionare il pacchetto **Grpc.Core** dalla scheda **Sfoglia** e fare clic su **Installa**.</span><span class="sxs-lookup"><span data-stu-id="2c78e-185">Select the **Grpc.Core** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="2c78e-186">Ripetere la procedura per `Google.Protobuf` e `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="2c78e-186">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2c78e-187">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2c78e-187">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="2c78e-188">Eseguire i comandi seguenti dal **terminale integrato**:</span><span class="sxs-lookup"><span data-stu-id="2c78e-188">Run the following commands from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Core
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2c78e-189">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="2c78e-189">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="2c78e-190">Fare clic con il pulsante destro del mouse sulla cartella **Pacchetti** nel **riquadro della soluzione** > **Aggiungi pacchetti**</span><span class="sxs-lookup"><span data-stu-id="2c78e-190">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="2c78e-191">Immettere **Grpc.Core** nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="2c78e-191">Enter **Grpc.Core** in the search box.</span></span>
* <span data-ttu-id="2c78e-192">Selezionare il pacchetto **Grpc.Core** nel riquadro dei risultati e fare clic su **Aggiungi pacchetto**</span><span class="sxs-lookup"><span data-stu-id="2c78e-192">Select the **Grpc.Core** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="2c78e-193">Ripetere la procedura per `Google.Protobuf` e `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="2c78e-193">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="2c78e-194">Aggiungere greet.proto</span><span class="sxs-lookup"><span data-stu-id="2c78e-194">Add greet.proto</span></span>

* <span data-ttu-id="2c78e-195">Creare una cartella **Protos** nel progetto client gRPC.</span><span class="sxs-lookup"><span data-stu-id="2c78e-195">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="2c78e-196">Copiare il file **Protos\greet.proto** dal servizio Greeter gRPC al progetto client gRPC.</span><span class="sxs-lookup"><span data-stu-id="2c78e-196">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="2c78e-197">Modificare il file di progetto *GrpcGreeterClient.csproj*:</span><span class="sxs-lookup"><span data-stu-id="2c78e-197">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2c78e-198">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2c78e-198">Visual Studio</span></span>](#tab/visual-studio) 

  <span data-ttu-id="2c78e-199">Fare clic con il pulsante destro del mouse sul progetto e selezionare **Modifica GrpcGreeterClient.csproj**.</span><span class="sxs-lookup"><span data-stu-id="2c78e-199">Right-click the project and select the **Edit GrpcGreeterClient.csproj**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2c78e-200">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2c78e-200">Visual Studio Code</span></span>](#tab/visual-studio-code) 

  <span data-ttu-id="2c78e-201">Selezionare il file *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="2c78e-201">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2c78e-202">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="2c78e-202">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="2c78e-203">Fare clic con il pulsante destro del mouse sul progetto e selezionare **Strumenti > Modifica File**.</span><span class="sxs-lookup"><span data-stu-id="2c78e-203">Right click the project and select **Tools > Edit File**.</span></span>

  ------

* <span data-ttu-id="2c78e-204">Aggiungere il file **greet.proto** al gruppo di elementi `<Protobuf>` del file di progetto GrpcGreeterClient:</span><span class="sxs-lookup"><span data-stu-id="2c78e-204">Add the **greet.proto** file to the `<Protobuf>` item group of the GrpcGreeterClient project file:</span></span>

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

<span data-ttu-id="2c78e-205">Compilare il progetto client per attivare la generazione degli asset client C#.</span><span class="sxs-lookup"><span data-stu-id="2c78e-205">Build the client project to trigger the generation of the C# client assets.</span></span>

### <a name="create-the-greater-client"></a><span data-ttu-id="2c78e-206">Creare il client Greeter</span><span class="sxs-lookup"><span data-stu-id="2c78e-206">Create the greater client</span></span>

<span data-ttu-id="2c78e-207">Compilare il progetto per creare i tipi nello spazio dei nomi **Greeter**.</span><span class="sxs-lookup"><span data-stu-id="2c78e-207">Build the project to create the types in the **Greeter** namespace.</span></span> <span data-ttu-id="2c78e-208">I tipi `Greeter` vengono generati automaticamente dal processo di compilazione.</span><span class="sxs-lookup"><span data-stu-id="2c78e-208">The `Greeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="2c78e-209">Aggiornare il file *Program.cs* del client gRPC con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="2c78e-209">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="2c78e-210">*Program.cs* contiene la logica e il punto di ingresso per il client gRPC.</span><span class="sxs-lookup"><span data-stu-id="2c78e-210">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="2c78e-211">Il client Greeter viene creato come descritto di seguito:</span><span class="sxs-lookup"><span data-stu-id="2c78e-211">The greater client is created by:</span></span>

* <span data-ttu-id="2c78e-212">Creare un'istanza di `Channel` contenente le informazioni per creare la connessione al servizio gRPC.</span><span class="sxs-lookup"><span data-stu-id="2c78e-212">Instantiating a `Channel` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="2c78e-213">Usare `Channel` per costruire il client Greeter:</span><span class="sxs-lookup"><span data-stu-id="2c78e-213">Using the `Channel` to construct the greater client:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=4-6)]

<span data-ttu-id="2c78e-214">Il client Greeter chiama il metodo `SayHello` asincrono.</span><span class="sxs-lookup"><span data-stu-id="2c78e-214">The greater client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="2c78e-215">Viene visualizzato il risultato della chiamata `SayHello`:</span><span class="sxs-lookup"><span data-stu-id="2c78e-215">The result of the `SayHello` call is displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-9)]

<span data-ttu-id="2c78e-216">Arrestare il `Channel` usato dal client al termine delle operazioni per rilasciare tutte le risorse.</span><span class="sxs-lookup"><span data-stu-id="2c78e-216">Shut down the `Channel` used by the client when operations have finished to release all resources.</span></span>

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="2c78e-217">Testare il client gRPC con il servizio Greeter gRPC</span><span class="sxs-lookup"><span data-stu-id="2c78e-217">Test the gRPC client with the gRPC Greeter service</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2c78e-218">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2c78e-218">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2c78e-219">Nel servizio Greeter premere CTRL+F5 per avviare il server senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="2c78e-219">In the Greeter service, press Ctrl+F5 to start the server without the debugger.</span></span>
* <span data-ttu-id="2c78e-220">Nel progetto GrpcGreeterClient premere CTRL+F5 per avviare il server senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="2c78e-220">In the GrpcGreeterClient project, press Ctrl+F5 to start the server without the debugger.</span></span>

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="2c78e-221">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="2c78e-221">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="2c78e-222">Avviare il servizio Greeter.</span><span class="sxs-lookup"><span data-stu-id="2c78e-222">Start the Greeter service.</span></span>
* <span data-ttu-id="2c78e-223">Avviare il client.</span><span class="sxs-lookup"><span data-stu-id="2c78e-223">Start the client.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="2c78e-224">Il client invia un saluto al servizio con un messaggio contenente il nome "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="2c78e-224">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="2c78e-225">Il servizio invia il messaggio "Hello GreeterClient" come risposta.</span><span class="sxs-lookup"><span data-stu-id="2c78e-225">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="2c78e-226">La risposta "Hello GreeterClient" viene visualizzata al prompt dei comandi:</span><span class="sxs-lookup"><span data-stu-id="2c78e-226">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="2c78e-227">Il servizio gRPC registra i dettagli della chiamata con esito positivo nei log scritti al prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="2c78e-227">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\GH\aspnet\docs\4\Docs\aspnetcore\tutorials\grpc\grpc-start\sample\GrpcGreeter
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 POST http://localhost:50051/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 78.32260000000001ms 200 application/grpc
```

### <a name="next-steps"></a><span data-ttu-id="2c78e-228">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="2c78e-228">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
