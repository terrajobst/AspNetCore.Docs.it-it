---
title: Creare un client e un server gRPC .NET Core in ASP.NET Core
author: juntaoluo
description: Questa esercitazione illustra come creare un servizio gRPC e un client gRPC in ASP.NET Core. Informazioni su come creare un progetto di servizio gRPC, modificare un file con estensione proto e aggiungere una chiamata con flusso bidirezionale.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 8/26/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 9eeb71ca751005780560f0f2200edc2013541c34
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925218"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="c8d1e-104">Esercitazione: Creare un client e un server gRPC in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c8d1e-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="c8d1e-105">A cura di [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="c8d1e-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="c8d1e-106">Questa esercitazione illustra come creare un client [gRPC](https://grpc.io/docs/guides/) .NET Core e un server gRPC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="c8d1e-107">Al termine, si otterrà un client gRPC che comunica con il servizio Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="c8d1e-108">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([procedura per il download](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="c8d1e-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="c8d1e-109">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="c8d1e-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c8d1e-110">Creare un server gRPC.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="c8d1e-111">Creare un client gRPC.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="c8d1e-112">Testare il servizio client gRPC con il servizio Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c8d1e-113">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="c8d1e-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c8d1e-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c8d1e-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c8d1e-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c8d1e-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c8d1e-116">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="c8d1e-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-grpc-service"></a><span data-ttu-id="c8d1e-117">Creare un servizio gRPC</span><span class="sxs-lookup"><span data-stu-id="c8d1e-117">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c8d1e-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c8d1e-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c8d1e-119">Avviare Visual Studio e selezionare **Crea un nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-119">Start Visual Studio and select **Create a new project**.</span></span> <span data-ttu-id="c8d1e-120">In alternativa, dal menu **File** di Visual Studio scegliere **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-120">Alternatively, from the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="c8d1e-121">Nella finestra di dialogo **Crea un nuovo progetto** selezionare **servizio GRPC** e selezionare **Avanti**:</span><span class="sxs-lookup"><span data-stu-id="c8d1e-121">In the **Create a new project** dialog, select **gRPC Service** and select **Next**:</span></span>

  ![Finestra di dialogo **Crea un nuovo progetto**](~/tutorials/grpc/grpc-start/static/cnp.png)

* <span data-ttu-id="c8d1e-123">Denominare il progetto **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-123">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="c8d1e-124">È importante denominare il progetto *GrpcGreeter* in modo che gli spazi dei nomi corrispondano nell'operazione copia/incolla del codice.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-124">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="c8d1e-125">Selezionare **Create**.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-125">Select **Create**.</span></span>
* <span data-ttu-id="c8d1e-126">Nella finestra di dialogo **Crea un nuovo servizio gRPC**:</span><span class="sxs-lookup"><span data-stu-id="c8d1e-126">In the **Create a new gRPC service** dialog:</span></span>
  * <span data-ttu-id="c8d1e-127">È selezionato il modello **Servizio gRPC**.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-127">The **gRPC Service** template is selected.</span></span>
  * <span data-ttu-id="c8d1e-128">Selezionare **Create**.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-128">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c8d1e-129">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c8d1e-129">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="c8d1e-130">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="c8d1e-130">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="c8d1e-131">Cambiare directory (`cd`) e passare alla cartella che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-131">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="c8d1e-132">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="c8d1e-132">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="c8d1e-133">Il comando `dotnet new` crea un nuovo servizio gRPC nella cartella *GrpcGreeter*.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-133">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="c8d1e-134">Il comando `code` apre la cartella *GrpcGreeter* in una nuova istanza di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-134">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="c8d1e-135">Viene visualizzata una finestra di dialogo con **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?** (Risorse di compilazione e debug mancanti da "RazorPagesMovie". Aggiungerle?)</span><span class="sxs-lookup"><span data-stu-id="c8d1e-135">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="c8d1e-136">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-136">Select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c8d1e-137">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="c8d1e-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="c8d1e-138">Da un terminale eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="c8d1e-138">From a terminal, run the following commands:</span></span>

```dotnetcli
dotnet new grpc -o GrpcGreeter
cd GrpcGreeter
```

<span data-ttu-id="c8d1e-139">I comandi precedenti usano l'[interfaccia della riga di comando di .NET Core](/dotnet/core/tools/dotnet) per creare un servizio gRPC.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-139">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="c8d1e-140">Aprire il progetto</span><span class="sxs-lookup"><span data-stu-id="c8d1e-140">Open the project</span></span>

<span data-ttu-id="c8d1e-141">In Visual Studio selezionare **File** > **Apri** e quindi selezionare il file *GrpcGreeter.sln*.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-141">From Visual Studio, select **File** > **Open**, and then select the *GrpcGreeter.sln* file.</span></span>

---

### <a name="run-the-service"></a><span data-ttu-id="c8d1e-142">Eseguire il servizio</span><span class="sxs-lookup"><span data-stu-id="c8d1e-142">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c8d1e-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c8d1e-143">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c8d1e-144">Premere `Ctrl+F5` per eseguire il servizio gRPC senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-144">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="c8d1e-145">Visual Studio esegue il servizio in un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-145">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c8d1e-146">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c8d1e-146">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="c8d1e-147">Eseguire il progetto gRPC Greeter *GrpcGreeter* dalla riga di comando tramite `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-147">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c8d1e-148">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="c8d1e-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="c8d1e-149">Eseguire il progetto gRPC Greeter *GrpcGreeter* dalla riga di comando tramite `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-149">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

---

<span data-ttu-id="c8d1e-150">I log indicano che il servizio è in ascolto su `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-150">The logs show the service listening on `https://localhost:5001`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

> [!NOTE]
> <span data-ttu-id="c8d1e-151">Il modello gRPC è configurato per l'uso di [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span><span class="sxs-lookup"><span data-stu-id="c8d1e-151">The gRPC template is configured to use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span> <span data-ttu-id="c8d1e-152">i client gRPC devono usare HTTPS per chiamare il server.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-152">gRPC clients need to use HTTPS to call the server.</span></span>
>
> <span data-ttu-id="c8d1e-153">macOS non supporta ASP.NET Core gRPC con TLS.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-153">macOS doesn't support ASP.NET Core gRPC with TLS.</span></span> <span data-ttu-id="c8d1e-154">Per eseguire correttamente i servizi gRPC in macOS, è necessaria una configurazione aggiuntiva.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-154">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="c8d1e-155">Per altre informazioni, vedere [Non è possibile avviare un'app ASP.NET Core gRPC in macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span><span class="sxs-lookup"><span data-stu-id="c8d1e-155">For more information, see [Unable to start ASP.NET Core gRPC app on macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span></span>

### <a name="examine-the-project-files"></a><span data-ttu-id="c8d1e-156">Esaminare i file di progetto</span><span class="sxs-lookup"><span data-stu-id="c8d1e-156">Examine the project files</span></span>

<span data-ttu-id="c8d1e-157">File di progetto *GrpcGreeter*:</span><span class="sxs-lookup"><span data-stu-id="c8d1e-157">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="c8d1e-158">*greet.proto* &ndash; Il file *Protos/greet.proto* definisce il gRPC `Greeter` e viene usato per generare gli asset del server gRPC.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-158">*greet.proto* &ndash; The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="c8d1e-159">Per altre informazioni, vedere [Introduzione a gRPC](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="c8d1e-159">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="c8d1e-160">Cartella *Servizi*: contiene l'implementazione del servizio `Greeter`.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-160">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="c8d1e-161">*appSettings.json* &ndash; Contiene i dati di configurazione, ad esempio il protocollo usato da Kestrel.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-161">*appSettings.json* &ndash; Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="c8d1e-162">Per altre informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-162">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="c8d1e-163">*Program.cs* &ndash; Contiene il punto di ingresso per il servizio gRPC.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-163">*Program.cs* &ndash; Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="c8d1e-164">Per altre informazioni, vedere <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-164">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="c8d1e-165">*Startup.cs* &ndash; Contiene il codice che configura il comportamento dell'app.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-165">*Startup.cs* &ndash; Contains code that configures app behavior.</span></span> <span data-ttu-id="c8d1e-166">Per altre informazioni, vedere [Avvio dell'app](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="c8d1e-166">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="c8d1e-167">Creare il client gRPC in un'app console .NET</span><span class="sxs-lookup"><span data-stu-id="c8d1e-167">Create the gRPC client in a .NET console app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c8d1e-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c8d1e-168">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c8d1e-169">Aprire una seconda istanza di Visual Studio e selezionare **Crea un nuovo progetto**.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-169">Open a second instance of Visual Studio and select **Create a new project**.</span></span>
* <span data-ttu-id="c8d1e-170">Nella finestra di dialogo **Crea un nuovo progetto** selezionare **App console (.NET Core)** e selezionare **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-170">In the **Create a new project** dialog, select **Console App (.NET Core)** and select **Next**.</span></span>
* <span data-ttu-id="c8d1e-171">Nella casella di testo **Nome** immettere **GrpcGreeterClient** e selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-171">In the **Name** text box, enter **GrpcGreeterClient** and select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c8d1e-172">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c8d1e-172">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="c8d1e-173">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="c8d1e-173">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="c8d1e-174">Cambiare directory (`cd`) e passare alla cartella che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-174">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="c8d1e-175">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="c8d1e-175">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new console -o GrpcGreeterClient
  code -r GrpcGreeterClient
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c8d1e-176">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="c8d1e-176">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="c8d1e-177">Seguire le istruzioni riportate in [Creazione di una soluzione .NET Core completa in macOS con Visual Studio per Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution) per creare un'app console con il nome *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-177">Follow the instructions in [Building a complete .NET Core solution on macOS using Visual Studio for Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

---

### <a name="add-required-packages"></a><span data-ttu-id="c8d1e-178">Aggiungere i pacchetti necessari</span><span class="sxs-lookup"><span data-stu-id="c8d1e-178">Add required packages</span></span>

<span data-ttu-id="c8d1e-179">Per il progetto client gRPC sono richiesti i pacchetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="c8d1e-179">The gRPC client project requires the following packages:</span></span>

* <span data-ttu-id="c8d1e-180">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), che contiene il client .NET Core.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-180">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="c8d1e-181">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), che contiene le API per i messaggi protobuf per C#.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-181">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="c8d1e-182">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), che contiene il supporto di strumenti C# per i file protobuf.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-182">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="c8d1e-183">Il pacchetto di strumenti non è necessario in fase di esecuzione, quindi la dipendenza è contrassegnata come `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-183">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c8d1e-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c8d1e-184">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c8d1e-185">Installare i pacchetti tramite la Console di gestione pacchetti (PMC) o Gestisci pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-185">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="c8d1e-186">Opzione con la console di Gestione pacchetti per installare i pacchetti</span><span class="sxs-lookup"><span data-stu-id="c8d1e-186">PMC option to install packages</span></span>

* <span data-ttu-id="c8d1e-187">Da Visual Studio selezionare **Strumenti** > **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="c8d1e-187">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="c8d1e-188">Dalla finestra **Console di Gestione pacchetti** eseguire `cd GrpcGreeterClient` per passare alla cartella contenente i file *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-188">From the **Package Manager Console** window, run `cd GrpcGreeterClient` to change directories to the folder containing the *GrpcGreeterClient.csproj* files.</span></span>
* <span data-ttu-id="c8d1e-189">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="c8d1e-189">Run the following commands:</span></span>

  ```powershell
  Install-Package Grpc.Net.Client
  Install-Package Google.Protobuf
  Install-Package Grpc.Tools
  ```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="c8d1e-190">Opzione Gestisci pacchetti NuGet per installare i pacchetti</span><span class="sxs-lookup"><span data-stu-id="c8d1e-190">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="c8d1e-191">Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** > **Gestisci pacchetti NuGet**</span><span class="sxs-lookup"><span data-stu-id="c8d1e-191">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="c8d1e-192">Selezionare la scheda **Sfoglia**.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-192">Select the **Browse** tab.</span></span>
* <span data-ttu-id="c8d1e-193">Immettere **Grpc.Net.Client** nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-193">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="c8d1e-194">Selezionare il pacchetto **Grpc.Net.Client** dalla scheda **Sfoglia** e fare clic su **Installa**.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-194">Select the **Grpc.Net.Client** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="c8d1e-195">Ripetere la procedura per `Google.Protobuf` e `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-195">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c8d1e-196">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c8d1e-196">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="c8d1e-197">Eseguire i comandi seguenti dal **terminale integrato**:</span><span class="sxs-lookup"><span data-stu-id="c8d1e-197">Run the following commands from the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet add GrpcGreeterClient.csproj package Grpc.Net.Client
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c8d1e-198">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="c8d1e-198">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="c8d1e-199">Fare clic con il pulsante destro del mouse sulla cartella **Pacchetti** nel **riquadro della soluzione** > **Aggiungi pacchetti**</span><span class="sxs-lookup"><span data-stu-id="c8d1e-199">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="c8d1e-200">Immettere **Grpc.Net.Client** nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-200">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="c8d1e-201">Selezionare il pacchetto **Grpc.Net.Client** nel riquadro dei risultati e fare clic su **Aggiungi pacchetto**</span><span class="sxs-lookup"><span data-stu-id="c8d1e-201">Select the **Grpc.Net.Client** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="c8d1e-202">Ripetere la procedura per `Google.Protobuf` e `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-202">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="c8d1e-203">Aggiungere greet.proto</span><span class="sxs-lookup"><span data-stu-id="c8d1e-203">Add greet.proto</span></span>

* <span data-ttu-id="c8d1e-204">Creare una cartella *Protos* nel progetto client gRPC.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-204">Create a *Protos* folder in the gRPC client project.</span></span>
* <span data-ttu-id="c8d1e-205">Copiare il file *Protos\greet.proto* dal servizio Greeter gRPC al progetto client gRPC.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-205">Copy the *Protos\greet.proto* file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="c8d1e-206">Modificare il file di progetto *GrpcGreeterClient.csproj*:</span><span class="sxs-lookup"><span data-stu-id="c8d1e-206">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c8d1e-207">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c8d1e-207">Visual Studio</span></span>](#tab/visual-studio)

  <span data-ttu-id="c8d1e-208">Fare clic con il pulsante destro del mouse sul progetto e scegliere **Modifica file di progetto**.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-208">Right-click the project and select **Edit Project File**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c8d1e-209">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c8d1e-209">Visual Studio Code</span></span>](#tab/visual-studio-code)

  <span data-ttu-id="c8d1e-210">Selezionare il file *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-210">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c8d1e-211">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="c8d1e-211">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="c8d1e-212">Fare clic con il pulsante destro del mouse sul progetto e scegliere **Strumenti** > **Modifica file**.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-212">Right-click the project and select **Tools** > **Edit File**.</span></span>

  ---

* <span data-ttu-id="c8d1e-213">Aggiungere un gruppo di elementi con un elemento `<Protobuf>` che fa riferimento al file *greet.proto*:</span><span class="sxs-lookup"><span data-stu-id="c8d1e-213">Add an item group with a `<Protobuf>` element that refers to the *greet.proto* file:</span></span>

  ```xml
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a><span data-ttu-id="c8d1e-214">Creare il client Greeter</span><span class="sxs-lookup"><span data-stu-id="c8d1e-214">Create the Greeter client</span></span>

<span data-ttu-id="c8d1e-215">Compilare il progetto per creare i tipi nello spazio dei nomi `GrpcGreeter`.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-215">Build the project to create the types in the `GrpcGreeter` namespace.</span></span> <span data-ttu-id="c8d1e-216">I tipi `GrpcGreeter` vengono generati automaticamente dal processo di compilazione.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-216">The `GrpcGreeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="c8d1e-217">Aggiornare il file *Program.cs* del client gRPC con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="c8d1e-217">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="c8d1e-218">*Program.cs* contiene la logica e il punto di ingresso per il client gRPC.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-218">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="c8d1e-219">Il client Greeter viene creato come descritto di seguito:</span><span class="sxs-lookup"><span data-stu-id="c8d1e-219">The Greeter client is created by:</span></span>

* <span data-ttu-id="c8d1e-220">Creare un'istanza di `HttpClient` contenente le informazioni per creare la connessione al servizio gRPC.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-220">Instantiating an `HttpClient` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="c8d1e-221">`HttpClient` Uso di per costruire un canale gRPC e il client greeter:</span><span class="sxs-lookup"><span data-stu-id="c8d1e-221">Using the `HttpClient` to construct a gRPC channel and the Greeter client:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=3-5)]

<span data-ttu-id="c8d1e-222">Il client Greeter chiama il metodo `SayHello` asincrono.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-222">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="c8d1e-223">Viene visualizzato il risultato della chiamata `SayHello`:</span><span class="sxs-lookup"><span data-stu-id="c8d1e-223">The result of the `SayHello` call is displayed:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=6-8)]

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="c8d1e-224">Testare il client gRPC con il servizio Greeter gRPC</span><span class="sxs-lookup"><span data-stu-id="c8d1e-224">Test the gRPC client with the gRPC Greeter service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c8d1e-225">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c8d1e-225">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c8d1e-226">Nel servizio Greeter premere `Ctrl+F5` per avviare il server senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-226">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="c8d1e-227">Nel progetto `GrpcGreeterClient` premere `Ctrl+F5` per avviare il client senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-227">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the client without the debugger.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c8d1e-228">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c8d1e-228">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="c8d1e-229">Avviare il servizio Greeter.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-229">Start the Greeter service.</span></span>
* <span data-ttu-id="c8d1e-230">Avviare il client.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-230">Start the client.</span></span>


# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c8d1e-231">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="c8d1e-231">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="c8d1e-232">Avviare il servizio Greeter.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-232">Start the Greeter service.</span></span>
* <span data-ttu-id="c8d1e-233">Avviare il client.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-233">Start the client.</span></span>

---

<span data-ttu-id="c8d1e-234">Il client invia un saluto al servizio con un messaggio contenente il nome *GreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-234">The client sends a greeting to the service with a message containing its name, *GreeterClient*.</span></span> <span data-ttu-id="c8d1e-235">Il servizio invia il messaggio "Hello GreeterClient" come risposta.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-235">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="c8d1e-236">La risposta "Hello GreeterClient" viene visualizzata al prompt dei comandi:</span><span class="sxs-lookup"><span data-stu-id="c8d1e-236">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="c8d1e-237">Il servizio gRPC registra i dettagli della chiamata con esito positivo nei log scritti al prompt dei comandi:</span><span class="sxs-lookup"><span data-stu-id="c8d1e-237">The gRPC service records the details of the successful call in the logs written to the command prompt:</span></span>

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

> [!NOTE]
> <span data-ttu-id="c8d1e-238">Il codice in questo articolo richiede il certificato di sviluppo HTTPS ASP.NET Core per proteggere il servizio gRPC.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-238">The code in this article requires the ASP.NET Core HTTPS development certificate to secure the gRPC service.</span></span> <span data-ttu-id="c8d1e-239">Se il client ha esito negativo con il messaggio `The remote certificate is invalid according to the validation procedure.`, il certificato di sviluppo non è attendibile.</span><span class="sxs-lookup"><span data-stu-id="c8d1e-239">If the client fails with the message `The remote certificate is invalid according to the validation procedure.`, the development certificate is not trusted.</span></span> <span data-ttu-id="c8d1e-240">Per istruzioni su come risolvere questo problema, vedere [Considerare attendibile il certificato di sviluppo di ASP.NET Core HTTPS in Windows e macOS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).</span><span class="sxs-lookup"><span data-stu-id="c8d1e-240">For instructions to fix this issue, see [Trust the ASP.NET Core HTTPS development certificate on Windows and macOS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).</span></span>

[!INCLUDE[](~/includes/gRPCazure.md)]

### <a name="next-steps"></a><span data-ttu-id="c8d1e-241">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="c8d1e-241">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
