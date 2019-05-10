---
title: 'Esercitazione: Creare un client gRPC .NET Core'
author: juntaoluo
description: Questa serie di esercitazioni illustra come creare un servizio gRPC in ASP.NET Core. Informazioni su come creare un progetto di servizio gRPC, modificare un file con estensione proto e aggiungere una chiamata con flusso bidirezionale.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 4/10/2019
uid: tutorials/grpc/grpc-client
ms.openlocfilehash: ec6bf5072c76de640a78b2c3f13dd1fc552b9d04
ms.sourcegitcommit: b508b115107e0f8d7f62b25cfcc8ad45e1373459
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/07/2019
ms.locfileid: "65212646"
---
# <a name="tutorial-create-a-net-core-grpc-client"></a><span data-ttu-id="b8686-104">Esercitazione: Creare un client gRPC .NET Core</span><span class="sxs-lookup"><span data-stu-id="b8686-104">Tutorial: Create a .NET Core gRPC client</span></span>

<span data-ttu-id="b8686-105">A cura di [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="b8686-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="b8686-106">Questa esercitazione illustra come creare un client [gRPC](https://grpc.io/docs/guides/) .NET Core in grado di comunicare con i servizi gRPC.</span><span class="sxs-lookup"><span data-stu-id="b8686-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client that can communicate with gRPC services.</span></span>

<span data-ttu-id="b8686-107">Al termine, si otterrà un client gRPC che comunica con il servizio Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="b8686-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

[!INCLUDE[View or download sample code](~/includes/grpc/downloadClient.md)]

<span data-ttu-id="b8686-108">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="b8686-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b8686-109">Creare un client gRPC.</span><span class="sxs-lookup"><span data-stu-id="b8686-109">Create a gRPC client.</span></span>
> * <span data-ttu-id="b8686-110">Eseguire il client sul servizio gRPC Greeter creato nell'esercitazione precedente.</span><span class="sxs-lookup"><span data-stu-id="b8686-110">Run the service against the gRPC Greeter service created in the previous tutorial.</span></span>
> * <span data-ttu-id="b8686-111">Esaminare i file di progetto.</span><span class="sxs-lookup"><span data-stu-id="b8686-111">Examine the project files.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-net-console-application"></a><span data-ttu-id="b8686-112">Creare un'applicazione console .NET</span><span class="sxs-lookup"><span data-stu-id="b8686-112">Create a .NET console application</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b8686-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b8686-113">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b8686-114">Seguire le istruzioni riportate [qui](/dotnet/core/tutorials/with-visual-studio) per creare un'app console con il nome *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="b8686-114">Follow the instructions [here](/dotnet/core/tutorials/with-visual-studio) to create a console app with the name *GrpcGreeterClient*.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b8686-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b8686-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="b8686-116">Seguire le istruzioni riportate [qui](/dotnet/core/tutorials/with-visual-studio-code) per creare un'app console con il nome *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="b8686-116">Follow the instructions [here](/dotnet/core/tutorials/with-visual-studio-code) to create a console app with the name *GrpcGreeterClient*.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b8686-117">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="b8686-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="b8686-118">Seguire le istruzioni riportate [qui](/dotnet/core/tutorials/using-on-mac-vs-full-solution) per creare un'app console con il nome *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="b8686-118">Follow the instructions [here](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

## <a name="add-required-packages"></a><span data-ttu-id="b8686-119">Aggiungere i pacchetti necessari</span><span class="sxs-lookup"><span data-stu-id="b8686-119">Add required packages</span></span>

<span data-ttu-id="b8686-120">Aggiungere i pacchetti seguenti nel progetto client gRPC:</span><span class="sxs-lookup"><span data-stu-id="b8686-120">Add the following packages to the gRPC client project:</span></span>

* <span data-ttu-id="b8686-121">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core), che contiene l'API C# per il client C-core.</span><span class="sxs-lookup"><span data-stu-id="b8686-121">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core), which contains the C# API for the C-core client.</span></span>
* <span data-ttu-id="b8686-122">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), che contiene le API per i messaggi protobuf per C#.</span><span class="sxs-lookup"><span data-stu-id="b8686-122">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="b8686-123">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), che contiene il supporto di strumenti C# per i file protobuf.</span><span class="sxs-lookup"><span data-stu-id="b8686-123">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="b8686-124">Il pacchetto di strumenti non è necessario in fase di esecuzione, quindi la dipendenza è contrassegnata come `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="b8686-124">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

<span data-ttu-id="b8686-125">È possibile aggiungere i pacchetti con gli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="b8686-125">Packages can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b8686-126">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b8686-126">Visual Studio</span></span>](#tab/visual-studio)

### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="b8686-127">Opzione con la console di Gestione pacchetti per installare i pacchetti</span><span class="sxs-lookup"><span data-stu-id="b8686-127">PMC option to install packages</span></span>

* <span data-ttu-id="b8686-128">Da Visual Studio selezionare **Strumenti** > **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="b8686-128">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="b8686-129">Dalla finestra **Console di Gestione pacchetti** passare alla directory in cui si trova il file *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="b8686-129">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="b8686-130">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="b8686-130">Run the following command:</span></span>

    ```powershell
    Install-Package Grpc.Core
    ```

* <span data-ttu-id="b8686-131">Ripetere `Install-Package` per Google.Protobuf e Grpc.Tools</span><span class="sxs-lookup"><span data-stu-id="b8686-131">Repeat the `Install-Package` for Google.Protobuf and Grpc.Tools</span></span>

<!-- Tutorials shouldn't have multiple options. Select what you think is the best approach. Recommend you removed this approach. -->

### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="b8686-132">Opzione Gestisci pacchetti NuGet per installare i pacchetti</span><span class="sxs-lookup"><span data-stu-id="b8686-132">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="b8686-133">Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** > **Gestisci pacchetti NuGet**</span><span class="sxs-lookup"><span data-stu-id="b8686-133">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="b8686-134">Impostare **Origine pacchetto** su "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="b8686-134">Set the **Package source** to "nuget.org"</span></span>
* <span data-ttu-id="b8686-135">Immettere "Grpc.Core" nella casella di ricerca</span><span class="sxs-lookup"><span data-stu-id="b8686-135">Enter "Grpc.Core" in the search box</span></span>
* <span data-ttu-id="b8686-136">Selezionare il pacchetto ".NET Core" dalla scheda **Sfoglia** e fare clic su **Installa**</span><span class="sxs-lookup"><span data-stu-id="b8686-136">Select the "Grpc.Core" package from the **Browse** tab and click **Install**</span></span>
* <span data-ttu-id="b8686-137">Ripetere per Google.Protobuf e Grpc.Tools</span><span class="sxs-lookup"><span data-stu-id="b8686-137">Repeat for Google.Protobuf and Grpc.Tools</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b8686-138">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b8686-138">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="b8686-139">Eseguire il comando seguente da **Terminale integrato**:</span><span class="sxs-lookup"><span data-stu-id="b8686-139">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Core
```

<span data-ttu-id="b8686-140">Ripetere per Google.Protobuf e Grpc.Tools</span><span class="sxs-lookup"><span data-stu-id="b8686-140">Repeat for Google.Protobuf and Grpc.Tools</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b8686-141">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="b8686-141">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="b8686-142">Fare clic su con il pulsante destro del mouse sulla cartella *Pacchetti* in **Solution Pad** (Riquadro della soluzione) > **Aggiungi pacchetti**</span><span class="sxs-lookup"><span data-stu-id="b8686-142">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="b8686-143">Impostare l'elenco a discesa **Aggiungi pacchetti** della finestra **Origine** su "nuget.org"</span><span class="sxs-lookup"><span data-stu-id="b8686-143">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="b8686-144">Immettere "Grpc.Core" nella casella di ricerca</span><span class="sxs-lookup"><span data-stu-id="b8686-144">Enter "Grpc.Core" in the search box</span></span>
* <span data-ttu-id="b8686-145">Selezionare il pacchetto "Grpc.Core" nel riquadro dei risultati e fare clic su **Aggiungi pacchetto**</span><span class="sxs-lookup"><span data-stu-id="b8686-145">Select the "Grpc.Core" package from the results pane and click **Add Package**</span></span>
* <span data-ttu-id="b8686-146">Ripetere per Google.Protobuf e Grpc.Tools</span><span class="sxs-lookup"><span data-stu-id="b8686-146">Repeat for Google.Protobuf and Grpc.Tools</span></span>

---

## <a name="add-the-greetproto-file"></a><span data-ttu-id="b8686-147">Aggiungere il file greet.proto</span><span class="sxs-lookup"><span data-stu-id="b8686-147">Add the greet.proto file</span></span>

<span data-ttu-id="b8686-148">Copiare il file **Protos\greet.proto** dal servizio Greeter gRPC al progetto client gRPC.</span><span class="sxs-lookup"><span data-stu-id="b8686-148">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span> <span data-ttu-id="b8686-149">Aggiungere il file **greet.proto** al gruppo di elementi `<Protobuf>` del file di progetto GrpcGreeterClient:</span><span class="sxs-lookup"><span data-stu-id="b8686-149">Add the **greet.proto** file to the `<Protobuf>` item group of the GrpcGreeterClient project file:</span></span>

```XML
<Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
```

> [!NOTE]
> <span data-ttu-id="b8686-150">Per aprire il file di progetto di GrpcGreeterClient, fare clic con il pulsante destro del mouse sul progetto e scegliere **Modifica GrpcGreeterClient.csproj** dal menu a discesa.</span><span class="sxs-lookup"><span data-stu-id="b8686-150">You can open the project file of GrpcGreeterClient by right-clicking the project and selecting the **Edit GrpcGreeterClient.csproj** option from the dropdown menu.</span></span>
>
> ![nuova applicazione Web ASP.NET Core](grpc-start/_static/edit_csproj.png)
>
> <span data-ttu-id="b8686-152">In alternativa, è possibile passare alla directory GrpcGreeterClient e modificare il file `GrpcGreeterClient.csproj` con l'editor preferito.</span><span class="sxs-lookup"><span data-stu-id="b8686-152">Alternatively, you can navigate to the GrpcGreeterClient directory and edit the `GrpcGreeterClient.csproj` with your favorite editor.</span></span>

<span data-ttu-id="b8686-153">L'attributo `GrpcServices="Client"` viene aggiunto in modo che vengano generati solo gli asset client C# per il file protobuf incluso.</span><span class="sxs-lookup"><span data-stu-id="b8686-153">The `GrpcServices="Client"` attribute is added so that only the C# client assets are generated for the included protobuf file.</span></span> <span data-ttu-id="b8686-154">Compilare il progetto client per attivare la generazione degli asset client C#.</span><span class="sxs-lookup"><span data-stu-id="b8686-154">Build the client project to trigger the generation of the C# client assets.</span></span>

## <a name="create-a-greeterclient-and-invoke-the-sayhello-unary-call"></a><span data-ttu-id="b8686-155">Creare un GreeterClient e richiamare la chiamata unaria SayHello</span><span class="sxs-lookup"><span data-stu-id="b8686-155">Create a GreeterClient and invoke the SayHello unary call</span></span>

<span data-ttu-id="b8686-156">Aggiungere il codice seguente al metodo `Main` del file `Program.cs` del progetto client gRPC:</span><span class="sxs-lookup"><span data-stu-id="b8686-156">Add the following code to `Main` method of the `Program.cs` file of the gRPC client project:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet)]

<span data-ttu-id="b8686-157">Per accedere ai tipi richiesti, sono necessarie le istruzioni using seguenti:</span><span class="sxs-lookup"><span data-stu-id="b8686-157">To access the required types the following using statements are required:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=using)]

<span data-ttu-id="b8686-158">GreeterClient viene creato creando un'istanza di `Channel` contenente le informazioni per la creazione della connessione al servizio gRPC e usandolo per costruire `GreeterClient`:</span><span class="sxs-lookup"><span data-stu-id="b8686-158">The GreeterClient is created by instantiating a `Channel` containing the information for creating the connection to the gRPC service and using it to construct the `GreeterClient`:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=4-5)]

<span data-ttu-id="b8686-159">GreeterClient contiene la chiamata unaria `SayHello` che può essere richiamata in modo asincrono:</span><span class="sxs-lookup"><span data-stu-id="b8686-159">The GreeterClient contains the unary call `SayHello` which can be invoked asynchronously:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-8)]

<span data-ttu-id="b8686-160">I risultati della chiamata `SayHello` vengono archiviati in `reply` e potranno poi essere visualizzati:</span><span class="sxs-lookup"><span data-stu-id="b8686-160">The results of the `SayHello` call is stored in `reply` which can then be displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=9)]

<span data-ttu-id="b8686-161">Il `Channel` usato dal client deve essere arrestato al termine delle operazioni per rilasciare tutte le risorse:</span><span class="sxs-lookup"><span data-stu-id="b8686-161">The `Channel` used by the client should be shut down when operations have finished to release all resources:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=11)]

> [!NOTE]
> <span data-ttu-id="b8686-162">È necessario compilare il progetto prima di poter risolvere i tipi nello spazio dei nomi **Greeter**.</span><span class="sxs-lookup"><span data-stu-id="b8686-162">You will need to build the project before the types in the **Greeter** namespace can be resolved.</span></span> <span data-ttu-id="b8686-163">Questi tipi vengono generati automaticamente durante la compilazione e non sono disponibili prima dell'esecuzione di una compilazione.</span><span class="sxs-lookup"><span data-stu-id="b8686-163">These types are generated automatically during build and are not be available before a build is run.</span></span>

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="b8686-164">Testare il client gRPC con il servizio Greeter gRPC</span><span class="sxs-lookup"><span data-stu-id="b8686-164">Test the gRPC client with the gRPC Greeter service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b8686-165">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b8686-165">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b8686-166">Verificare che il servizio Greeter creato nell'esercitazione precedente sia in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="b8686-166">Ensure the Greeter service created in the previous tutorial is running.</span></span>

* <span data-ttu-id="b8686-167">Quando il servizio è in esecuzione, tornare al progetto **GrpcGreeterClient** e impostarlo come progetto di avvio.</span><span class="sxs-lookup"><span data-stu-id="b8686-167">Once the service is running, return to the **GrpcGreeterClient** project set it as the Startup Project.</span></span> <span data-ttu-id="b8686-168">Premere CTRL+F5 per eseguire il client senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="b8686-168">Press Ctrl+F5 to run the client without the debugger.</span></span>

  <span data-ttu-id="b8686-169">Il client invia un saluto al servizio con un messaggio contenente il nome "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="b8686-169">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="b8686-170">Il servizio invierà un messaggio "Hello GreeterClient" come risposta visualizzata al prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="b8686-170">The service will send a message "Hello GreeterClient" as a response that is displayed in the command prompt.</span></span>

  ![nuova applicazione Web ASP.NET Core](grpc-start/_static/client.png)

  <span data-ttu-id="b8686-172">Il servizio registra i dettagli della chiamata con esito positivo nei log scritti al prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="b8686-172">The service records the details of the successful call in the logs written to the command prompt.</span></span>

  ![nuova applicazione Web ASP.NET Core](grpc-start/_static/server_complete.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="b8686-174">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="b8686-174">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="b8686-175">Verificare che il servizio Greeter creato nell'esercitazione precedente sia in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="b8686-175">Ensure the Greeter service created in the previous tutorial is running.</span></span>

* <span data-ttu-id="b8686-176">Eseguire il progetto client GrpcGreeter.Client dalla riga di comando separata tramite `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="b8686-176">Run the Client project GrpcGreeter.Client from the separate command line using `dotnet run`.</span></span>

<span data-ttu-id="b8686-177">Il client invia un saluto al servizio con un messaggio contenente il nome "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="b8686-177">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="b8686-178">Il servizio invierà un messaggio "Hello GreeterClient" come risposta visualizzata al prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="b8686-178">The service will send a message "Hello GreeterClient" as a response that is displayed in the command prompt.</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="b8686-179">Il servizio registra i dettagli della chiamata con esito positivo nei log scritti al prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="b8686-179">The service records the details of the successful call in the logs written to the command prompt.</span></span>

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
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[1]
      Request starting HTTP/2 POST http://localhost:50051/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[2]
      Request finished in 194.5798ms 200 application/grpc
```

<!-- End of combined VS/Mac tabs -->

---

### <a name="examine-the-project-files-of-the-grpc-project"></a><span data-ttu-id="b8686-180">Esaminare i file di progetto del progetto gRPC</span><span class="sxs-lookup"><span data-stu-id="b8686-180">Examine the project files of the gRPC project</span></span>

<span data-ttu-id="b8686-181">File GrpcGreeterClient del client gRPC:</span><span class="sxs-lookup"><span data-stu-id="b8686-181">gRPC client GrpcGreeterClient file:</span></span>

<span data-ttu-id="b8686-182">*Program.cs* contiene la logica e il punto di ingresso per il client gRPC.</span><span class="sxs-lookup"><span data-stu-id="b8686-182">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b8686-183">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="b8686-183">Additional resources</span></span>

<span data-ttu-id="b8686-184">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="b8686-184">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b8686-185">Creare un client gRPC.</span><span class="sxs-lookup"><span data-stu-id="b8686-185">Create a gRPC client.</span></span>
> * <span data-ttu-id="b8686-186">Eseguire il client sul servizio gRPC Greeter creato nell'esercitazione precedente.</span><span class="sxs-lookup"><span data-stu-id="b8686-186">Run the service against the gRPC Greeter service created in the previous tutorial.</span></span>
> * <span data-ttu-id="b8686-187">Esaminare i file di progetto.</span><span class="sxs-lookup"><span data-stu-id="b8686-187">Examine the project files.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b8686-188">Precedente: Creare un servizio Greeter gRPC</span><span class="sxs-lookup"><span data-stu-id="b8686-188">Previous: Create a gRPC Greeter Service</span></span>](xref:tutorials/grpc/grpc-start)
