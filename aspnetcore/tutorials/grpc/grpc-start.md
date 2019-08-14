---
title: Creare un client e un server gRPC .NET Core in ASP.NET Core
author: juntaoluo
description: Questa esercitazione illustra come creare un servizio gRPC e un client gRPC in ASP.NET Core. Informazioni su come creare un progetto di servizio gRPC, modificare un file con estensione proto e aggiungere una chiamata con flusso bidirezionale.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 06/12/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 6a3a7446a488ef54d99d6c7605980c18890b9ad0
ms.sourcegitcommit: 4fe3ae892f54dc540859bff78741a28c2daa9a38
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/04/2019
ms.locfileid: "68776634"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="3c85f-104">Esercitazione: Creare un client e un server gRPC in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3c85f-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="3c85f-105">A cura di [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="3c85f-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="3c85f-106">Questa esercitazione illustra come creare un client [gRPC](https://grpc.io/docs/guides/) .NET Core e un server gRPC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3c85f-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="3c85f-107">Al termine, si otterrà un client gRPC che comunica con il servizio Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="3c85f-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="3c85f-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="3c85f-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="3c85f-109">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="3c85f-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3c85f-110">Creare un server gRPC.</span><span class="sxs-lookup"><span data-stu-id="3c85f-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="3c85f-111">Creare un client gRPC.</span><span class="sxs-lookup"><span data-stu-id="3c85f-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="3c85f-112">Testare il servizio client gRPC con il servizio Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="3c85f-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3c85f-113">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="3c85f-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c85f-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c85f-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3c85f-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3c85f-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3c85f-116">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="3c85f-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-grpc-service"></a><span data-ttu-id="3c85f-117">Creare un servizio gRPC</span><span class="sxs-lookup"><span data-stu-id="3c85f-117">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c85f-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c85f-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3c85f-119">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="3c85f-119">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="3c85f-120">Nella finestra di dialogo **Crea un nuovo progetto** selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="3c85f-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="3c85f-121">Scegliere **Avanti**</span><span class="sxs-lookup"><span data-stu-id="3c85f-121">Select **Next**</span></span>
* <span data-ttu-id="3c85f-122">Denominare il progetto **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="3c85f-122">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="3c85f-123">È importante denominare il progetto *GrpcGreeter* in modo che gli spazi dei nomi corrispondano nell'operazione copia/incolla del codice.</span><span class="sxs-lookup"><span data-stu-id="3c85f-123">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="3c85f-124">Selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="3c85f-124">Select **Create**</span></span>
* <span data-ttu-id="3c85f-125">Nella finestra di dialogo **Crea una nuova applicazione Web ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="3c85f-125">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="3c85f-126">Selezionare **.NET Core** e **ASP.NET Core 3.0** negli elenchi a discesa.</span><span class="sxs-lookup"><span data-stu-id="3c85f-126">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="3c85f-127">Selezionare il modello del **servizio gRPC**.</span><span class="sxs-lookup"><span data-stu-id="3c85f-127">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="3c85f-128">Selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="3c85f-128">Select **Create**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3c85f-129">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3c85f-129">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="3c85f-130">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="3c85f-130">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="3c85f-131">Cambiare directory (`cd`) e passare alla cartella che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="3c85f-131">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="3c85f-132">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="3c85f-132">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="3c85f-133">Il comando `dotnet new` crea un nuovo servizio gRPC nella cartella *GrpcGreeter*.</span><span class="sxs-lookup"><span data-stu-id="3c85f-133">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="3c85f-134">Il comando `code` apre la cartella *GrpcGreeter* in una nuova istanza di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="3c85f-134">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="3c85f-135">Viene visualizzata una finestra di dialogo con **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?** (Risorse di compilazione e debug mancanti da "RazorPagesMovie". Aggiungerle?)</span><span class="sxs-lookup"><span data-stu-id="3c85f-135">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="3c85f-136">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="3c85f-136">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3c85f-137">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="3c85f-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="3c85f-138">Da un terminale eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="3c85f-138">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="3c85f-139">I comandi precedenti usano l'[interfaccia della riga di comando di .NET Core](/dotnet/core/tools/dotnet) per creare un servizio gRPC.</span><span class="sxs-lookup"><span data-stu-id="3c85f-139">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="3c85f-140">Aprire il progetto</span><span class="sxs-lookup"><span data-stu-id="3c85f-140">Open the project</span></span>

<span data-ttu-id="3c85f-141">In Visual Studio selezionare **File > Apri** e quindi selezionare il file *GrpcGreeter.sln*.</span><span class="sxs-lookup"><span data-stu-id="3c85f-141">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="3c85f-142">Eseguire il servizio</span><span class="sxs-lookup"><span data-stu-id="3c85f-142">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c85f-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c85f-143">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3c85f-144">Premere `Ctrl+F5` per eseguire il servizio gRPC senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="3c85f-144">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="3c85f-145">Visual Studio esegue il servizio in un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="3c85f-145">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="3c85f-146">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="3c85f-146">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="3c85f-147">Eseguire il progetto gRPC Greeter *GrpcGreeter* dalla riga di comando tramite `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="3c85f-147">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="3c85f-148">I log indicano che il servizio è in ascolto su `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="3c85f-148">The logs show the service listening on `https://localhost:5001`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

### <a name="examine-the-project-files"></a><span data-ttu-id="3c85f-149">Esaminare i file di progetto</span><span class="sxs-lookup"><span data-stu-id="3c85f-149">Examine the project files</span></span>

<span data-ttu-id="3c85f-150">File di progetto *GrpcGreeter*:</span><span class="sxs-lookup"><span data-stu-id="3c85f-150">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="3c85f-151">*greet.proto*: Il file *Protos/greet.proto* definisce il gRPC `Greeter` e viene usato per generare le risorse del server gRPC.</span><span class="sxs-lookup"><span data-stu-id="3c85f-151">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="3c85f-152">Per altre informazioni, vedere [Introduzione a gRPC](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="3c85f-152">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="3c85f-153">Cartella *Servizi*: contiene l'implementazione del servizio `Greeter`.</span><span class="sxs-lookup"><span data-stu-id="3c85f-153">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="3c85f-154">*appSettings.json*: contiene i dati di configurazione, ad esempio il protocollo usato da Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3c85f-154">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="3c85f-155">Per altre informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="3c85f-155">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="3c85f-156">*Program.cs*: Contiene il punto di ingresso per il servizio gRPC.</span><span class="sxs-lookup"><span data-stu-id="3c85f-156">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="3c85f-157">Per altre informazioni, vedere <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="3c85f-157">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="3c85f-158">*Startup.cs*: Contiene codice che consente di configurare il comportamento dell'app.</span><span class="sxs-lookup"><span data-stu-id="3c85f-158">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="3c85f-159">Per altre informazioni, vedere [Avvio dell'app](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="3c85f-159">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="3c85f-160">Creare il client gRPC in un'app console .NET</span><span class="sxs-lookup"><span data-stu-id="3c85f-160">Create the gRPC client in a .NET console app</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c85f-161">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c85f-161">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3c85f-162">Aprire una seconda istanza di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3c85f-162">Open a second instance of Visual Studio.</span></span>
* <span data-ttu-id="3c85f-163">Selezionare **File** > **Nuovo** > **Progetto** dalla barra dei menu.</span><span class="sxs-lookup"><span data-stu-id="3c85f-163">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="3c85f-164">Nella finestra di dialogo **Crea un nuovo progetto** selezionare **App console (.NET Core)** .</span><span class="sxs-lookup"><span data-stu-id="3c85f-164">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="3c85f-165">Scegliere **Avanti**</span><span class="sxs-lookup"><span data-stu-id="3c85f-165">Select **Next**</span></span>
* <span data-ttu-id="3c85f-166">Nella casella di testo **Nome** immettere "GrpcGreeterClient".</span><span class="sxs-lookup"><span data-stu-id="3c85f-166">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="3c85f-167">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="3c85f-167">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3c85f-168">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3c85f-168">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="3c85f-169">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="3c85f-169">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="3c85f-170">Cambiare directory (`cd`) e passare alla cartella che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="3c85f-170">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="3c85f-171">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="3c85f-171">Run the following commands:</span></span>

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3c85f-172">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="3c85f-172">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="3c85f-173">Seguire le istruzioni riportate [qui](/dotnet/core/tutorials/using-on-mac-vs-full-solution) per creare un'app console con il nome *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="3c85f-173">Follow the instructions [here](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a><span data-ttu-id="3c85f-174">Aggiungere i pacchetti necessari</span><span class="sxs-lookup"><span data-stu-id="3c85f-174">Add required packages</span></span>

<span data-ttu-id="3c85f-175">Per il progetto client gRPC sono richiesti i pacchetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="3c85f-175">The gRPC client project requires the following packages:</span></span>

* <span data-ttu-id="3c85f-176">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), che contiene il client .NET Core.</span><span class="sxs-lookup"><span data-stu-id="3c85f-176">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="3c85f-177">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), che contiene le API per i messaggi protobuf per C#.</span><span class="sxs-lookup"><span data-stu-id="3c85f-177">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="3c85f-178">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), che contiene il supporto di strumenti C# per i file protobuf.</span><span class="sxs-lookup"><span data-stu-id="3c85f-178">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="3c85f-179">Il pacchetto di strumenti non è necessario in fase di esecuzione, quindi la dipendenza è contrassegnata come `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="3c85f-179">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c85f-180">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c85f-180">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="3c85f-181">Installare i pacchetti tramite la Console di gestione pacchetti (PMC) o Gestisci pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="3c85f-181">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="3c85f-182">Opzione con la console di Gestione pacchetti per installare i pacchetti</span><span class="sxs-lookup"><span data-stu-id="3c85f-182">PMC option to install packages</span></span>

* <span data-ttu-id="3c85f-183">Da Visual Studio selezionare **Strumenti** > **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="3c85f-183">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="3c85f-184">Dalla finestra **Console di Gestione pacchetti** passare alla directory in cui si trova il file *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="3c85f-184">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="3c85f-185">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="3c85f-185">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="3c85f-186">Opzione Gestisci pacchetti NuGet per installare i pacchetti</span><span class="sxs-lookup"><span data-stu-id="3c85f-186">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="3c85f-187">Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** > **Gestisci pacchetti NuGet**</span><span class="sxs-lookup"><span data-stu-id="3c85f-187">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="3c85f-188">Selezionare la scheda **Sfoglia**.</span><span class="sxs-lookup"><span data-stu-id="3c85f-188">Select the **Browse** tab.</span></span>
* <span data-ttu-id="3c85f-189">Immettere **Grpc.Net.Client** nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="3c85f-189">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="3c85f-190">Selezionare il pacchetto **Grpc.Net.Client** dalla scheda **Sfoglia** e fare clic su **Installa**.</span><span class="sxs-lookup"><span data-stu-id="3c85f-190">Select the **Grpc.Net.Client** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="3c85f-191">Ripetere la procedura per `Google.Protobuf` e `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="3c85f-191">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3c85f-192">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3c85f-192">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="3c85f-193">Eseguire i comandi seguenti dal **terminale integrato**:</span><span class="sxs-lookup"><span data-stu-id="3c85f-193">Run the following commands from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Net.Client
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3c85f-194">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="3c85f-194">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="3c85f-195">Fare clic con il pulsante destro del mouse sulla cartella **Pacchetti** nel **riquadro della soluzione** > **Aggiungi pacchetti**</span><span class="sxs-lookup"><span data-stu-id="3c85f-195">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="3c85f-196">Immettere **Grpc.Net.Client** nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="3c85f-196">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="3c85f-197">Selezionare il pacchetto **Grpc.Net.Client** nel riquadro dei risultati e fare clic su **Aggiungi pacchetto**</span><span class="sxs-lookup"><span data-stu-id="3c85f-197">Select the **Grpc.Net.Client** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="3c85f-198">Ripetere la procedura per `Google.Protobuf` e `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="3c85f-198">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="3c85f-199">Aggiungere greet.proto</span><span class="sxs-lookup"><span data-stu-id="3c85f-199">Add greet.proto</span></span>

* <span data-ttu-id="3c85f-200">Creare una cartella **Protos** nel progetto client gRPC.</span><span class="sxs-lookup"><span data-stu-id="3c85f-200">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="3c85f-201">Copiare il file **Protos\greet.proto** dal servizio Greeter gRPC al progetto client gRPC.</span><span class="sxs-lookup"><span data-stu-id="3c85f-201">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="3c85f-202">Modificare il file di progetto *GrpcGreeterClient.csproj*:</span><span class="sxs-lookup"><span data-stu-id="3c85f-202">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c85f-203">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c85f-203">Visual Studio</span></span>](#tab/visual-studio) 

  <span data-ttu-id="3c85f-204">Fare clic con il pulsante destro del mouse sul progetto e scegliere **Modifica file di progetto**.</span><span class="sxs-lookup"><span data-stu-id="3c85f-204">Right-click the project and select **Edit Project File**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3c85f-205">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3c85f-205">Visual Studio Code</span></span>](#tab/visual-studio-code) 

  <span data-ttu-id="3c85f-206">Selezionare il file *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="3c85f-206">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3c85f-207">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="3c85f-207">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="3c85f-208">Fare clic con il pulsante destro del mouse sul progetto e scegliere **Strumenti > Modifica file**.</span><span class="sxs-lookup"><span data-stu-id="3c85f-208">Right-click the project and select **Tools > Edit File**.</span></span>

  ---

* <span data-ttu-id="3c85f-209">Aggiungere un gruppo di elementi con un elemento `<Protobuf>` che fa riferimento al file **greet.proto**:</span><span class="sxs-lookup"><span data-stu-id="3c85f-209">Add an item group with a `<Protobuf>` element that refers to the **greet.proto** file:</span></span>

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a><span data-ttu-id="3c85f-210">Creare il client Greeter</span><span class="sxs-lookup"><span data-stu-id="3c85f-210">Create the Greeter client</span></span>

<span data-ttu-id="3c85f-211">Compilare il progetto per creare i tipi nello spazio dei nomi `GrpcGreeter`.</span><span class="sxs-lookup"><span data-stu-id="3c85f-211">Build the project to create the types in the `GrpcGreeter` namespace.</span></span> <span data-ttu-id="3c85f-212">I tipi `GrpcGreeter` vengono generati automaticamente dal processo di compilazione.</span><span class="sxs-lookup"><span data-stu-id="3c85f-212">The `GrpcGreeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="3c85f-213">Aggiornare il file *Program.cs* del client gRPC con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="3c85f-213">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="3c85f-214">*Program.cs* contiene la logica e il punto di ingresso per il client gRPC.</span><span class="sxs-lookup"><span data-stu-id="3c85f-214">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="3c85f-215">Il client Greeter viene creato come descritto di seguito:</span><span class="sxs-lookup"><span data-stu-id="3c85f-215">The Greeter client is created by:</span></span>

* <span data-ttu-id="3c85f-216">Creare un'istanza di `HttpClient` contenente le informazioni per creare la connessione al servizio gRPC.</span><span class="sxs-lookup"><span data-stu-id="3c85f-216">Instantiating an `HttpClient` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="3c85f-217">Usare `HttpClient` per costruire il client Greeter:</span><span class="sxs-lookup"><span data-stu-id="3c85f-217">Using the `HttpClient` to construct the Greeter client:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=3-6)]

<span data-ttu-id="3c85f-218">Il client Greeter chiama il metodo `SayHello` asincrono.</span><span class="sxs-lookup"><span data-stu-id="3c85f-218">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="3c85f-219">Viene visualizzato il risultato della chiamata `SayHello`:</span><span class="sxs-lookup"><span data-stu-id="3c85f-219">The result of the `SayHello` call is displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-9)]

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="3c85f-220">Testare il client gRPC con il servizio Greeter gRPC</span><span class="sxs-lookup"><span data-stu-id="3c85f-220">Test the gRPC client with the gRPC Greeter service</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c85f-221">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c85f-221">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3c85f-222">Nel servizio Greeter premere `Ctrl+F5` per avviare il server senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="3c85f-222">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="3c85f-223">Nel progetto `GrpcGreeterClient` premere `Ctrl+F5` per avviare il client senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="3c85f-223">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the client without the debugger.</span></span>

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="3c85f-224">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="3c85f-224">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="3c85f-225">Avviare il servizio Greeter.</span><span class="sxs-lookup"><span data-stu-id="3c85f-225">Start the Greeter service.</span></span>
* <span data-ttu-id="3c85f-226">Avviare il client.</span><span class="sxs-lookup"><span data-stu-id="3c85f-226">Start the client.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="3c85f-227">Il client invia un saluto al servizio con un messaggio contenente il nome "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="3c85f-227">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="3c85f-228">Il servizio invia il messaggio "Hello GreeterClient" come risposta.</span><span class="sxs-lookup"><span data-stu-id="3c85f-228">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="3c85f-229">La risposta "Hello GreeterClient" viene visualizzata al prompt dei comandi:</span><span class="sxs-lookup"><span data-stu-id="3c85f-229">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="3c85f-230">Il servizio gRPC registra i dettagli della chiamata con esito positivo nei log scritti al prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="3c85f-230">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\GH\aspnet\docs\4\Docs\aspnetcore\tutorials\grpc\grpc-start\sample\GrpcGreeter
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 POST https://localhost:5001/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 78.32260000000001ms 200 application/grpc
```

### <a name="next-steps"></a><span data-ttu-id="3c85f-231">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="3c85f-231">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
