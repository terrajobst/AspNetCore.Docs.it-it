---
title: "Esercitazione: Introduzione all'uso di gRPC in ASP.NET Core"
author: juntaoluo
description: Questa serie di esercitazioni illustra come creare un servizio gRPC in ASP.NET Core. Informazioni su come creare un progetto di servizio gRPC, modificare un file con estensione proto e aggiungere una chiamata con flusso bidirezionale.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 2/26/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: a67050331cc8563b1baf5312b92910c1bbdfbd69
ms.sourcegitcommit: 57a974556acd09363a58f38c26f74dc21e0d4339
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59672567"
---
# <a name="tutorial-get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="09178-104">Esercitazione: Introduzione all'uso del servizio gRPC in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="09178-104">Tutorial: Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="09178-105">A cura di [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="09178-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="09178-106">Questa esercitazione illustra le nozioni di base della creazione di un servizio gRPC in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="09178-106">This tutorial teaches the basics of building a gRPC service on ASP.NET Core.</span></span>

<span data-ttu-id="09178-107">Al termine dell'esercitazione si avrà un servizio gRPC che restituisce messaggi di apertura.</span><span class="sxs-lookup"><span data-stu-id="09178-107">At the end, you'll have a gRPC service that echoes greetings.</span></span>

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

<span data-ttu-id="09178-108">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="09178-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="09178-109">Creare un servizio gRPC.</span><span class="sxs-lookup"><span data-stu-id="09178-109">Create a gRPC service.</span></span>
> * <span data-ttu-id="09178-110">Eseguire il servizio gRPC.</span><span class="sxs-lookup"><span data-stu-id="09178-110">Run the gRPC service.</span></span>
> * <span data-ttu-id="09178-111">Esaminare i file di progetto.</span><span class="sxs-lookup"><span data-stu-id="09178-111">Examine the project files.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a><span data-ttu-id="09178-112">Creare un servizio gRPC</span><span class="sxs-lookup"><span data-stu-id="09178-112">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="09178-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="09178-113">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="09178-114">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="09178-114">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="09178-115">Creare una nuova applicazione Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="09178-115">Create a new ASP.NET Core Web Application.</span></span>
  <span data-ttu-id="09178-116">![nuova applicazione Web ASP.NET Core](grpc-start/_static/np_3_0.1.png)</span><span class="sxs-lookup"><span data-stu-id="09178-116">![new ASP.NET Core Web Application](grpc-start/_static/np_3_0.1.png)</span></span>
* <span data-ttu-id="09178-117">Denominare il progetto **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="09178-117">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="09178-118">È importante denominare il progetto *GrpcGreeter* in modo che gli spazi dei nomi corrispondano nell'operazione copia/incolla del codice.</span><span class="sxs-lookup"><span data-stu-id="09178-118">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="09178-119">![nuova applicazione Web ASP.NET Core](grpc-start/_static/np_3_0.2.png)</span><span class="sxs-lookup"><span data-stu-id="09178-119">![new ASP.NET Core Web Application](grpc-start/_static/np_3_0.2.png)</span></span>
* <span data-ttu-id="09178-120">Selezionare **.NET Core** e **ASP.NET Core 3.0** nell'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="09178-120">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown.</span></span> <span data-ttu-id="09178-121">Scegliere il modello del **servizio gRPC**.</span><span class="sxs-lookup"><span data-stu-id="09178-121">Choose the **gRPC Service** template.</span></span>

  <span data-ttu-id="09178-122">Viene creato il progetto iniziale seguente:</span><span class="sxs-lookup"><span data-stu-id="09178-122">The following starter project is created:</span></span>

  ![Esplora soluzioni](grpc-start/_static/se3.0.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="09178-124">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="09178-124">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="09178-125">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="09178-125">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="09178-126">Cambiare directory (`cd`) e passare alla cartella che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="09178-126">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="09178-127">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="09178-127">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="09178-128">Il comando `dotnet new` crea un nuovo servizio gRPC nella cartella *GrpcGreeter*.</span><span class="sxs-lookup"><span data-stu-id="09178-128">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="09178-129">Il comando `code` apre la cartella *GrpcGreeter* in una nuova istanza di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="09178-129">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="09178-130">Viene visualizzata una finestra di dialogo con **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?** (Risorse di compilazione e debug mancanti da "RazorPagesMovie". Aggiungerle?)</span><span class="sxs-lookup"><span data-stu-id="09178-130">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="09178-131">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="09178-131">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="09178-132">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="09178-132">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="09178-133">Da un terminale eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="09178-133">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="09178-134">I comandi precedenti usano l'[interfaccia della riga di comando di .NET Core](/dotnet/core/tools/dotnet) per creare un servizio gRPC.</span><span class="sxs-lookup"><span data-stu-id="09178-134">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="09178-135">Aprire il progetto</span><span class="sxs-lookup"><span data-stu-id="09178-135">Open the project</span></span>

<span data-ttu-id="09178-136">In Visual Studio selezionare **File > Apri** e quindi selezionare il file *GrpcGreeter.sln*.</span><span class="sxs-lookup"><span data-stu-id="09178-136">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="09178-137">Eseguire il servizio</span><span class="sxs-lookup"><span data-stu-id="09178-137">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="09178-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="09178-138">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="09178-139">Premere CTRL+F5 per eseguire il servizio gRPC senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="09178-139">Press Ctrl+F5 to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="09178-140">Visual Studio esegue il servizio in un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="09178-140">Visual Studio runs the service in a command prompt.</span></span> <span data-ttu-id="09178-141">I log mostrano che il servizio ha avviato l'ascolto su `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="09178-141">The logs show that the service started listening on `http://localhost:50051`.</span></span>

  ![nuova applicazione Web ASP.NET Core](grpc-start/_static/server_start.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="09178-143">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="09178-143">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="09178-144">Eseguire il progetto gRPC Greeter GrpcGreeter dalla riga di comando tramite `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="09178-144">Run the gRPC Greeter project GrpcGreeter from the command line using `dotnet run`.</span></span> <span data-ttu-id="09178-145">I log mostrano che il servizio ha avviato l'ascolto su `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="09178-145">The logs show that the service started listening on `http://localhost:50051`.</span></span>

```console
dbug: Grpc.AspNetCore.Server.Internal.GrpcServiceBinder[1]
      Added gRPC method 'SayHello' to service 'Greet.Greeter'. Method type: 'Unary', route pattern: '/Greet.Greeter/SayHello'.
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\gh\Docs\aspnetcore\tutorials\grpc\grpc-start\samples\GrpcGreeter
```

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="09178-146">L'esercitazione successiva illustra come creare un client gRPC, necessario per testare il servizio Greeter.</span><span class="sxs-lookup"><span data-stu-id="09178-146">The next tutorial will demonstrate how to build a gRPC client, which is required to test the Greeter service.</span></span>

### <a name="examine-the-project-files-of-the-grpc-project"></a><span data-ttu-id="09178-147">Esaminare i file di progetto del progetto gRPC</span><span class="sxs-lookup"><span data-stu-id="09178-147">Examine the project files of the gRPC project</span></span>

<span data-ttu-id="09178-148">File GrpcGreeter:</span><span class="sxs-lookup"><span data-stu-id="09178-148">GrpcGreeter files:</span></span>

* <span data-ttu-id="09178-149">greet.proto: Il file *Protos/greet.proto* definisce il gRPC `Greeter` e viene usato per generare le risorse del server gRPC.</span><span class="sxs-lookup"><span data-stu-id="09178-149">greet.proto: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="09178-150">Per ulteriori informazioni, vedere <xref:grpc/index>.</span><span class="sxs-lookup"><span data-stu-id="09178-150">For more information, see <xref:grpc/index>.</span></span>
* <span data-ttu-id="09178-151">Cartella *Servizi*: contiene l'implementazione del servizio `Greeter`.</span><span class="sxs-lookup"><span data-stu-id="09178-151">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="09178-152">*appSettings. JSON*: contiene i dati di configurazione, ad esempio il protocollo usato da Kestrel.</span><span class="sxs-lookup"><span data-stu-id="09178-152">*appSettings.json*:Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="09178-153">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="09178-153">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="09178-154">*Program.cs*: Contiene il punto di ingresso per il servizio gRPC.</span><span class="sxs-lookup"><span data-stu-id="09178-154">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="09178-155">Per ulteriori informazioni, vedere <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="09178-155">For more information, see <xref:fundamentals/host/web-host>.</span></span>
* <span data-ttu-id="09178-156">Startup.cs: Contiene codice che consente di configurare il comportamento dell'app.</span><span class="sxs-lookup"><span data-stu-id="09178-156">Startup.cs: Contains code that configures app behavior.</span></span> <span data-ttu-id="09178-157">Per ulteriori informazioni, vedere <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="09178-157">For more information, see <xref:fundamentals/startup>.</span></span>

### <a name="test-the-service"></a><span data-ttu-id="09178-158">Eseguire il test del servizio</span><span class="sxs-lookup"><span data-stu-id="09178-158">Test the service</span></span>

## <a name="additional-resources"></a><span data-ttu-id="09178-159">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="09178-159">Additional resources</span></span>

<span data-ttu-id="09178-160">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="09178-160">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="09178-161">È stato creato un servizio gRPC.</span><span class="sxs-lookup"><span data-stu-id="09178-161">Created a gRPC service.</span></span>
> * <span data-ttu-id="09178-162">È stato eseguito il servizio gRPC.</span><span class="sxs-lookup"><span data-stu-id="09178-162">Ran the gRPC service.</span></span>
> * <span data-ttu-id="09178-163">Esame dei file di progetto.</span><span class="sxs-lookup"><span data-stu-id="09178-163">Examined the project files.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="09178-164">Successivo: Creare un client gRPC .NET Core</span><span class="sxs-lookup"><span data-stu-id="09178-164">Next: Create a .NET Core gRPC client</span></span>](xref:tutorials/grpc/grpc-client)