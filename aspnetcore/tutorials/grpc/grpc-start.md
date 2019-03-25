---
title: "Esercitazione: Introduzione all'uso di gRPC in ASP.NET Core"
author: juntaoluo
description: Questa serie di esercitazioni illustra come creare un servizio gRPC in ASP.NET Core. Informazioni su come creare un progetto di servizio gRPC, modificare un file con estensione proto e aggiungere una chiamata con flusso bidirezionale.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 2/26/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 5f7a2f6b57804b3295b23c322dcbac553b05528b
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320056"
---
# <a name="tutorial-get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="a9f62-104">Esercitazione: Introduzione all'uso del servizio gRPC in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a9f62-104">Tutorial: Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="a9f62-105">A cura di [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="a9f62-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="a9f62-106">Questa esercitazione illustra le nozioni di base della creazione di un servizio gRPC in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a9f62-106">This tutorial teaches the basics of building a gRPC service on ASP.NET Core.</span></span>

<span data-ttu-id="a9f62-107">Al termine dell'esercitazione si avrà un servizio gRPC che restituisce messaggi di apertura.</span><span class="sxs-lookup"><span data-stu-id="a9f62-107">At the end, you'll have a gRPC service that echoes greetings.</span></span>

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

<span data-ttu-id="a9f62-108">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="a9f62-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a9f62-109">Creare un servizio gRPC.</span><span class="sxs-lookup"><span data-stu-id="a9f62-109">Create a gRPC service.</span></span>
> * <span data-ttu-id="a9f62-110">Eseguire il servizio.</span><span class="sxs-lookup"><span data-stu-id="a9f62-110">Run the service.</span></span>
> * <span data-ttu-id="a9f62-111">Esaminare i file di progetto.</span><span class="sxs-lookup"><span data-stu-id="a9f62-111">Examine the project files.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a><span data-ttu-id="a9f62-112">Creare un servizio gRPC</span><span class="sxs-lookup"><span data-stu-id="a9f62-112">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a9f62-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9f62-113">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a9f62-114">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="a9f62-114">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="a9f62-115">Creare una nuova applicazione Web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a9f62-115">Create a new ASP.NET Core Web Application.</span></span>
  <span data-ttu-id="a9f62-116">![nuova applicazione Web ASP.NET Core](grpc-start/_static/np_3_0.1.png)</span><span class="sxs-lookup"><span data-stu-id="a9f62-116">![new ASP.NET Core Web Application](grpc-start/_static/np_3_0.1.png)</span></span>
* <span data-ttu-id="a9f62-117">Denominare il progetto **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="a9f62-117">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="a9f62-118">È importante denominare il progetto *GrpcGreeter* in modo che gli spazi dei nomi corrispondano nell'operazione copia/incolla del codice.</span><span class="sxs-lookup"><span data-stu-id="a9f62-118">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="a9f62-119">![nuova applicazione Web ASP.NET Core](grpc-start/_static/np_3_0.2.png)</span><span class="sxs-lookup"><span data-stu-id="a9f62-119">![new ASP.NET Core Web Application](grpc-start/_static/np_3_0.2.png)</span></span>
* <span data-ttu-id="a9f62-120">Selezionare **.NET Core** e **ASP.NET Core 3.0** nell'elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="a9f62-120">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown.</span></span> <span data-ttu-id="a9f62-121">Scegliere il modello del **servizio gRPC**.</span><span class="sxs-lookup"><span data-stu-id="a9f62-121">Choose the **gRPC Service** template.</span></span>

  <span data-ttu-id="a9f62-122">Viene creato il progetto iniziale seguente:</span><span class="sxs-lookup"><span data-stu-id="a9f62-122">The following starter project is created:</span></span>

  ![Esplora soluzioni](grpc-start/_static/se3.0.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a9f62-124">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a9f62-124">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a9f62-125">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="a9f62-125">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="a9f62-126">Cambiare directory (`cd`) e passare alla cartella che conterrà il progetto.</span><span class="sxs-lookup"><span data-stu-id="a9f62-126">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="a9f62-127">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="a9f62-127">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="a9f62-128">Il comando `dotnet new` crea un nuovo servizio gRPC nella cartella *GrpcGreeter*.</span><span class="sxs-lookup"><span data-stu-id="a9f62-128">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="a9f62-129">Il comando `code` apre la cartella *GrpcGreeter* in una nuova istanza di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="a9f62-129">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="a9f62-130">Viene visualizzata una finestra di dialogo con **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?** (Risorse di compilazione e debug mancanti da "RazorPagesMovie". Aggiungerle?)</span><span class="sxs-lookup"><span data-stu-id="a9f62-130">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="a9f62-131">Selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="a9f62-131">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a9f62-132">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="a9f62-132">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="a9f62-133">Da un terminale eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="a9f62-133">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="a9f62-134">I comandi precedenti usano l'[interfaccia della riga di comando di .NET Core](/dotnet/core/tools/dotnet) per creare un servizio gRPC.</span><span class="sxs-lookup"><span data-stu-id="a9f62-134">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="a9f62-135">Aprire il progetto</span><span class="sxs-lookup"><span data-stu-id="a9f62-135">Open the project</span></span>

<span data-ttu-id="a9f62-136">In Visual Studio selezionare **File > Apri** e quindi selezionare il file *GrpcGreeter.sln*.</span><span class="sxs-lookup"><span data-stu-id="a9f62-136">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="test-the-service"></a><span data-ttu-id="a9f62-137">Eseguire il test del servizio</span><span class="sxs-lookup"><span data-stu-id="a9f62-137">Test the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a9f62-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a9f62-138">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a9f62-139">Verificare che il **GrpcGreeter.Server** sia impostato come progetto di avvio e premere CTRL+F5 per eseguire il servizio gRPC senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="a9f62-139">Ensure the **GrpcGreeter.Server** is set as the Startup Project and press Ctrl+F5 to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="a9f62-140">Visual Studio esegue il servizio in un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="a9f62-140">Visual Studio runs the service in a command prompt.</span></span> <span data-ttu-id="a9f62-141">I log mostrano che il servizio ha avviato l'ascolto su `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="a9f62-141">The logs show that the service started listening on `http://localhost:50051`.</span></span>

  ![nuova applicazione Web ASP.NET Core](grpc-start/_static/server_start.png)

* <span data-ttu-id="a9f62-143">Quando il servizio è in esecuzione, verificare che il **GrpcGreeter.Client** sia impostato come progetto di avvio e premere CTRL+F5 per eseguire il client senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="a9f62-143">Once the service is running, set the **GrpcGreeter.Client** is set as the Startup Project and press Ctrl+F5 to run the client without the debugger.</span></span>

  <span data-ttu-id="a9f62-144">Il client invia un saluto al servizio con un messaggio contenente il nome "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="a9f62-144">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="a9f62-145">Il servizio invierà un messaggio "Hello GreeterClient" come risposta visualizzata al prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="a9f62-145">The service will send a message "Hello GreeterClient" as a response that is displayed in the command prompt.</span></span>

  ![nuova applicazione Web ASP.NET Core](grpc-start/_static/client.png)

  <span data-ttu-id="a9f62-147">Il servizio registra i dettagli della chiamata con esito positivo nei log scritti al prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="a9f62-147">The service records the details of the successful call in the logs written to the command prompt.</span></span>

  ![nuova applicazione Web ASP.NET Core](grpc-start/_static/server_complete.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="a9f62-149">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="a9f62-149">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="a9f62-150">Eseguire il progetto server GrpcGreeter.Server dalla riga di comando tramite `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="a9f62-150">Run the Server project GrpcGreeter.Server from the command line using `dotnet run`.</span></span> <span data-ttu-id="a9f62-151">I log mostrano che il servizio ha avviato l'ascolto su `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="a9f62-151">The logs show that the service started listening on `http://localhost:50051`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\example\GrpcGreeter\GrpcGreeter.Server
```

* <span data-ttu-id="a9f62-152">Eseguire il progetto client GrpcGreeter.Client dalla riga di comando separata tramite `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="a9f62-152">Run the Client project GrpcGreeter.Client from the separate command line using `dotnet run`.</span></span>

<span data-ttu-id="a9f62-153">Il client invia un saluto al servizio con un messaggio contenente il nome "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="a9f62-153">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="a9f62-154">Il servizio invierà un messaggio "Hello GreeterClient" come risposta visualizzata al prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="a9f62-154">The service will send a message "Hello GreeterClient" as a response that is displayed in the command prompt.</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="a9f62-155">Il servizio registra i dettagli della chiamata con esito positivo nei log scritti al prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="a9f62-155">The service records the details of the successful call in the logs written to the command prompt.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\gh\tp\GrpcGreeter\GrpcGreeter.Server
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[1]
      Request starting HTTP/2 POST http://localhost:50051/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[2]
      Request finished in 107.46730000000001ms 200 application/grpc
```

<!-- End of combined VS/Mac tabs -->

---

### <a name="examine-the-project-files-of-the-grpc-project"></a><span data-ttu-id="a9f62-156">Esaminare i file di progetto del progetto gRPC</span><span class="sxs-lookup"><span data-stu-id="a9f62-156">Examine the project files of the gRPC project</span></span>

<span data-ttu-id="a9f62-157">File GrpcGreeter.Server:</span><span class="sxs-lookup"><span data-stu-id="a9f62-157">GrpcGreeter.Server files:</span></span>

* <span data-ttu-id="a9f62-158">greet.proto: Il file *Protos/greet.proto* definisce il gRPC `Greeter` e viene usato per generare le risorse del server gRPC.</span><span class="sxs-lookup"><span data-stu-id="a9f62-158">greet.proto: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="a9f62-159">Per ulteriori informazioni, vedere <xref:grpc/index>.</span><span class="sxs-lookup"><span data-stu-id="a9f62-159">For more information, see <xref:grpc/index>.</span></span>
* <span data-ttu-id="a9f62-160">Cartella *Servizi*: contiene l'implementazione del servizio `Greeter`.</span><span class="sxs-lookup"><span data-stu-id="a9f62-160">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="a9f62-161">*appSettings. JSON*: contiene i dati di configurazione, ad esempio il protocollo usato da Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a9f62-161">*appSettings.json*:Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="a9f62-162">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="a9f62-162">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="a9f62-163">*Program.cs*: Contiene il punto di ingresso per il servizio gRPC.</span><span class="sxs-lookup"><span data-stu-id="a9f62-163">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="a9f62-164">Per ulteriori informazioni, vedere <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="a9f62-164">For more information, see <xref:fundamentals/host/web-host>.</span></span>
* <span data-ttu-id="a9f62-165">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="a9f62-165">Startup.cs</span></span>

<span data-ttu-id="a9f62-166">Contiene codice che consente di configurare il comportamento dell'app.</span><span class="sxs-lookup"><span data-stu-id="a9f62-166">Contains code that configures app behavior.</span></span> <span data-ttu-id="a9f62-167">Per ulteriori informazioni, vedere <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="a9f62-167">For more information, see <xref:fundamentals/startup>.</span></span>

<span data-ttu-id="a9f62-168">File GrpcGreeter.Client del client gRPC:</span><span class="sxs-lookup"><span data-stu-id="a9f62-168">gRPC client GrpcGreeter.Client file:</span></span>

<span data-ttu-id="a9f62-169">*Program.cs* contiene la logica e il punto di ingresso per il client gRPC.</span><span class="sxs-lookup"><span data-stu-id="a9f62-169">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="a9f62-170">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="a9f62-170">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a9f62-171">È stato creato un servizio gRPC.</span><span class="sxs-lookup"><span data-stu-id="a9f62-171">Created a gRPC service.</span></span>
> * <span data-ttu-id="a9f62-172">Sono stati eseguiti il servizio e un client per testare il servizio.</span><span class="sxs-lookup"><span data-stu-id="a9f62-172">Ran the service and a client to test the service.</span></span>
> * <span data-ttu-id="a9f62-173">Esame dei file di progetto.</span><span class="sxs-lookup"><span data-stu-id="a9f62-173">Examined the project files.</span></span>
