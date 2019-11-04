---
page_type: sample
description: Questa esercitazione illustra come creare un servizio gRPC e un client gRPC in ASP.NET Core.
languages:
- csharp
products:
- dotnet-core
- aspnet-core
- vs
urlFragment: create-grpc-client
ms.openlocfilehash: b9feb9eed62177358fffc0d7da582f625a431e32
ms.sourcegitcommit: 9e85c2562df5e108d7933635c830297f484bb775
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/04/2019
ms.locfileid: "73463041"
---
# <a name="create-a-grpc-client-and-server-in-aspnet-core-30-using-visual-studio"></a><span data-ttu-id="83bad-102">Creare un client e un server gRPC in ASP.NET Core 3.0 con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="83bad-102">Create a gRPC client and server in ASP.NET Core 3.0 using Visual Studio</span></span>

<span data-ttu-id="83bad-103">Questa esercitazione illustra come creare un client [gRPC](https://grpc.io/docs/guides/) .NET Core e un server gRPC ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="83bad-103">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="83bad-104">Al termine, si otterrà un client gRPC che comunica con il servizio Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="83bad-104">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="83bad-105">Le attività di questa esercitazione sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="83bad-105">In this tutorial, you;</span></span>

* <span data-ttu-id="83bad-106">Creare un server gRPC.</span><span class="sxs-lookup"><span data-stu-id="83bad-106">Create a gRPC Server.</span></span>
* <span data-ttu-id="83bad-107">Creare un client gRPC.</span><span class="sxs-lookup"><span data-stu-id="83bad-107">Create a gRPC client.</span></span>
* <span data-ttu-id="83bad-108">Testare il servizio client gRPC con il servizio Greeter gRPC.</span><span class="sxs-lookup"><span data-stu-id="83bad-108">Test the gRPC client service with the gRPC Greeter service.</span></span>

## <a name="create-a-grpc-service"></a><span data-ttu-id="83bad-109">Creare un servizio gRPC</span><span class="sxs-lookup"><span data-stu-id="83bad-109">Create a gRPC service</span></span>

* <span data-ttu-id="83bad-110">Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="83bad-110">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="83bad-111">Nella finestra di dialogo **Crea un nuovo progetto** selezionare **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="83bad-111">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="83bad-112">Scegliere **Avanti**</span><span class="sxs-lookup"><span data-stu-id="83bad-112">Select **Next**</span></span>
* <span data-ttu-id="83bad-113">Denominare il progetto **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="83bad-113">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="83bad-114">È importante denominare il progetto *GrpcGreeter* in modo che gli spazi dei nomi corrispondano nell'operazione copia/incolla del codice.</span><span class="sxs-lookup"><span data-stu-id="83bad-114">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="83bad-115">Selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="83bad-115">Select **Create**</span></span>
* <span data-ttu-id="83bad-116">Nella finestra di dialogo **Crea una nuova applicazione Web ASP.NET Core**:</span><span class="sxs-lookup"><span data-stu-id="83bad-116">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="83bad-117">Selezionare **.NET Core** e **ASP.NET Core 3.0** negli elenchi a discesa.</span><span class="sxs-lookup"><span data-stu-id="83bad-117">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="83bad-118">Selezionare il modello del **servizio gRPC**.</span><span class="sxs-lookup"><span data-stu-id="83bad-118">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="83bad-119">Selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="83bad-119">Select **Create**</span></span>

### <a name="run-the-service"></a><span data-ttu-id="83bad-120">Eseguire il servizio</span><span class="sxs-lookup"><span data-stu-id="83bad-120">Run the service</span></span>

* <span data-ttu-id="83bad-121">Premere `Ctrl+F5` per eseguire il servizio gRPC senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="83bad-121">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="83bad-122">Visual Studio esegue il servizio in un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="83bad-122">Visual Studio runs the service in a command prompt.</span></span>

<span data-ttu-id="83bad-123">I log indicano che il servizio è in ascolto su `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="83bad-123">The logs show the service listening on `https://localhost:5001`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

> [!NOTE]
> <span data-ttu-id="83bad-124">Il modello gRPC è configurato per l'uso di [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span><span class="sxs-lookup"><span data-stu-id="83bad-124">The gRPC template is configured to use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span> <span data-ttu-id="83bad-125">i client gRPC devono usare HTTPS per chiamare il server.</span><span class="sxs-lookup"><span data-stu-id="83bad-125">gRPC clients need to use HTTPS to call the server.</span></span>
>
> <span data-ttu-id="83bad-126">macOS non supporta ASP.NET Core gRPC con TLS.</span><span class="sxs-lookup"><span data-stu-id="83bad-126">macOS doesn't support ASP.NET Core gRPC with TLS.</span></span> <span data-ttu-id="83bad-127">Per eseguire correttamente i servizi gRPC in macOS, è necessaria una configurazione aggiuntiva.</span><span class="sxs-lookup"><span data-stu-id="83bad-127">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="83bad-128">Per altre informazioni, vedere [gRPC e ASP.NET Core in macOS](xref:grpc/aspnetcore#grpc-and-aspnet-core-on-macos).</span><span class="sxs-lookup"><span data-stu-id="83bad-128">For more information, see [gRPC and ASP.NET Core on macOS](xref:grpc/aspnetcore#grpc-and-aspnet-core-on-macos).</span></span>

### <a name="examine-the-project-files"></a><span data-ttu-id="83bad-129">Esaminare i file di progetto</span><span class="sxs-lookup"><span data-stu-id="83bad-129">Examine the project files</span></span>

<span data-ttu-id="83bad-130">File di progetto *GrpcGreeter*:</span><span class="sxs-lookup"><span data-stu-id="83bad-130">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="83bad-131">*greet. proto*: il file *Protos/greet. proto* definisce il `Greeter` gRPC e viene usato per generare gli asset del server gRPC.</span><span class="sxs-lookup"><span data-stu-id="83bad-131">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="83bad-132">Per altre informazioni, vedere [Introduzione a gRPC](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="83bad-132">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="83bad-133">Cartella dei *Servizi* : contiene l'implementazione del servizio `Greeter`.</span><span class="sxs-lookup"><span data-stu-id="83bad-133">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="83bad-134">*appSettings. JSON*: contiene i dati di configurazione, ad esempio il protocollo usato da gheppio.</span><span class="sxs-lookup"><span data-stu-id="83bad-134">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="83bad-135">Per ulteriori informazioni, vedere <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="83bad-135">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="83bad-136">*Program.cs*: contiene il punto di ingresso per il servizio gRPC.</span><span class="sxs-lookup"><span data-stu-id="83bad-136">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="83bad-137">Per ulteriori informazioni, vedere <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="83bad-137">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="83bad-138">*Startup.cs*: contiene il codice che configura il comportamento dell'app.</span><span class="sxs-lookup"><span data-stu-id="83bad-138">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="83bad-139">Per altre informazioni, vedere [Avvio dell'app](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="83bad-139">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="83bad-140">Creare il client gRPC in un'app console .NET</span><span class="sxs-lookup"><span data-stu-id="83bad-140">Create the gRPC client in a .NET console app</span></span>

* <span data-ttu-id="83bad-141">Aprire una seconda istanza di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="83bad-141">Open a second instance of Visual Studio.</span></span>
* <span data-ttu-id="83bad-142">Selezionare **File** > **Nuovo** > **Progetto** dalla barra dei menu.</span><span class="sxs-lookup"><span data-stu-id="83bad-142">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="83bad-143">Nella finestra di dialogo **Crea un nuovo progetto** selezionare **App console (.NET Core)** .</span><span class="sxs-lookup"><span data-stu-id="83bad-143">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="83bad-144">Scegliere **Avanti**</span><span class="sxs-lookup"><span data-stu-id="83bad-144">Select **Next**</span></span>
* <span data-ttu-id="83bad-145">Nella casella di testo **Nome** immettere "GrpcGreeterClient".</span><span class="sxs-lookup"><span data-stu-id="83bad-145">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="83bad-146">Scegliere **Crea**.</span><span class="sxs-lookup"><span data-stu-id="83bad-146">Select **Create**.</span></span>

### <a name="add-required-packages"></a><span data-ttu-id="83bad-147">Aggiungere i pacchetti necessari</span><span class="sxs-lookup"><span data-stu-id="83bad-147">Add required packages</span></span>

<span data-ttu-id="83bad-148">Per il progetto client gRPC sono richiesti i pacchetti seguenti:</span><span class="sxs-lookup"><span data-stu-id="83bad-148">The gRPC client project requires the following packages:</span></span>

* <span data-ttu-id="83bad-149">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), che contiene il client .NET Core.</span><span class="sxs-lookup"><span data-stu-id="83bad-149">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="83bad-150">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), che contiene le API per i messaggi protobuf per C#.</span><span class="sxs-lookup"><span data-stu-id="83bad-150">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="83bad-151">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), che contiene il supporto di strumenti C# per i file protobuf.</span><span class="sxs-lookup"><span data-stu-id="83bad-151">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="83bad-152">Il pacchetto di strumenti non è necessario in fase di esecuzione, quindi la dipendenza è contrassegnata come `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="83bad-152">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

<span data-ttu-id="83bad-153">Installare i pacchetti tramite la Console di gestione pacchetti (PMC) o Gestisci pacchetti NuGet.</span><span class="sxs-lookup"><span data-stu-id="83bad-153">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="83bad-154">Opzione con la console di Gestione pacchetti per installare i pacchetti</span><span class="sxs-lookup"><span data-stu-id="83bad-154">PMC option to install packages</span></span>

* <span data-ttu-id="83bad-155">Da Visual Studio selezionare **Strumenti** > **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**</span><span class="sxs-lookup"><span data-stu-id="83bad-155">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="83bad-156">Dalla finestra **Console di Gestione pacchetti** passare alla directory in cui si trova il file *GrpcGreeterClient.csproj*.</span><span class="sxs-lookup"><span data-stu-id="83bad-156">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="83bad-157">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="83bad-157">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="83bad-158">Opzione Gestisci pacchetti NuGet per installare i pacchetti</span><span class="sxs-lookup"><span data-stu-id="83bad-158">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="83bad-159">Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** > **Gestisci pacchetti NuGet**</span><span class="sxs-lookup"><span data-stu-id="83bad-159">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="83bad-160">Selezionare la scheda **Sfoglia**.</span><span class="sxs-lookup"><span data-stu-id="83bad-160">Select the **Browse** tab.</span></span>
* <span data-ttu-id="83bad-161">Immettere **Grpc.Net.Client** nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="83bad-161">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="83bad-162">Selezionare il pacchetto **Grpc.Net.Client** dalla scheda **Sfoglia** e fare clic su **Installa**.</span><span class="sxs-lookup"><span data-stu-id="83bad-162">Select the **Grpc.Net.Client** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="83bad-163">Ripetere la procedura per `Google.Protobuf` e `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="83bad-163">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="add-greetproto"></a><span data-ttu-id="83bad-164">Aggiungere greet.proto</span><span class="sxs-lookup"><span data-stu-id="83bad-164">Add greet.proto</span></span>

* <span data-ttu-id="83bad-165">Creare una cartella **Protos** nel progetto client gRPC.</span><span class="sxs-lookup"><span data-stu-id="83bad-165">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="83bad-166">Copiare il file **Protos\greet.proto** dal servizio Greeter gRPC al progetto client gRPC.</span><span class="sxs-lookup"><span data-stu-id="83bad-166">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="83bad-167">Modificare il file di progetto *GrpcGreeterClient.csproj*:</span><span class="sxs-lookup"><span data-stu-id="83bad-167">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  <span data-ttu-id="83bad-168">Fare clic con il pulsante destro del mouse sul progetto e scegliere **Modifica file di progetto**.</span><span class="sxs-lookup"><span data-stu-id="83bad-168">Right-click the project and select **Edit Project File**.</span></span>

* <span data-ttu-id="83bad-169">Aggiungere un gruppo di elementi con un elemento `<Protobuf>` che fa riferimento al file **greet.proto**:</span><span class="sxs-lookup"><span data-stu-id="83bad-169">Add an item group with a `<Protobuf>` element that refers to the **greet.proto** file:</span></span>

  ```xml
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a><span data-ttu-id="83bad-170">Creare il client Greeter</span><span class="sxs-lookup"><span data-stu-id="83bad-170">Create the Greeter client</span></span>

<span data-ttu-id="83bad-171">Compilare il progetto per creare i tipi nello spazio dei nomi `GrpcGreeter`.</span><span class="sxs-lookup"><span data-stu-id="83bad-171">Build the project to create the types in the `GrpcGreeter` namespace.</span></span> <span data-ttu-id="83bad-172">I tipi `GrpcGreeter` vengono generati automaticamente dal processo di compilazione.</span><span class="sxs-lookup"><span data-stu-id="83bad-172">The `GrpcGreeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="83bad-173">Aggiornare il file *Program.cs* del client gRPC con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="83bad-173">Update the gRPC client *Program.cs* file with the following code:</span></span>

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;
using GrpcGreeter;
using Grpc.Net.Client;

namespace GrpcGreeterClient
{
    class Program
    {
        static async Task Main(string[] args)
        {
            // The port number(5001) must match the port of the gRPC server.
            var channel = GrpcChannel.ForAddress("https://localhost:5001");
            var client = new Greeter.GreeterClient(channel);
            var reply = await client.SayHelloAsync(
                              new HelloRequest { Name = "GreeterClient" });
            Console.WriteLine("Greeting: " + reply.Message);
            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

<span data-ttu-id="83bad-174">*Program.cs* contiene la logica e il punto di ingresso per il client gRPC.</span><span class="sxs-lookup"><span data-stu-id="83bad-174">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="83bad-175">Il client Greeter viene creato come descritto di seguito:</span><span class="sxs-lookup"><span data-stu-id="83bad-175">The Greeter client is created by:</span></span>

* <span data-ttu-id="83bad-176">Creare un'istanza di `GrpcChannel` contenente le informazioni per creare la connessione al servizio gRPC.</span><span class="sxs-lookup"><span data-stu-id="83bad-176">Instantiating a `GrpcChannel` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="83bad-177">Uso del `GrpcChannel` per costruire il client Greeter.</span><span class="sxs-lookup"><span data-stu-id="83bad-177">Using the `GrpcChannel` to construct the Greeter client.</span></span>

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="83bad-178">Testare il client gRPC con il servizio Greeter gRPC</span><span class="sxs-lookup"><span data-stu-id="83bad-178">Test the gRPC client with the gRPC Greeter service</span></span>

* <span data-ttu-id="83bad-179">Nel servizio Greeter premere `Ctrl+F5` per avviare il server senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="83bad-179">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="83bad-180">Nel progetto `GrpcGreeterClient` premere `Ctrl+F5` per avviare il client senza il debugger.</span><span class="sxs-lookup"><span data-stu-id="83bad-180">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the client without the debugger.</span></span>

<span data-ttu-id="83bad-181">Il client invia un saluto al servizio con un messaggio contenente il nome "GreeterClient".</span><span class="sxs-lookup"><span data-stu-id="83bad-181">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="83bad-182">Il servizio invia il messaggio "Hello GreeterClient" come risposta.</span><span class="sxs-lookup"><span data-stu-id="83bad-182">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="83bad-183">La risposta "Hello GreeterClient" viene visualizzata al prompt dei comandi:</span><span class="sxs-lookup"><span data-stu-id="83bad-183">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="83bad-184">Il servizio gRPC registra i dettagli della chiamata con esito positivo nei log scritti al prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="83bad-184">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

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

### <a name="docs-help--next-steps-for-grpc"></a><span data-ttu-id="83bad-185">Guida di docs e passaggi successivi per gRPC</span><span class="sxs-lookup"><span data-stu-id="83bad-185">Docs help & next steps for gRPC</span></span>

* [<span data-ttu-id="83bad-186">Introduzione a gRPC su ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="83bad-186">Introduction to gRPC on ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/grpc/index?view=aspnetcore-3.0)
* [<span data-ttu-id="83bad-187">Servizi gRPC con C#</span><span class="sxs-lookup"><span data-stu-id="83bad-187">gRPC services with C#</span></span>](https://docs.microsoft.com/aspnet/core/grpc/basics?view=aspnetcore-3.0)
* [<span data-ttu-id="83bad-188">Migrazione di servizi gRPC da C-core ad ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="83bad-188">Migrating gRPC services from C-core to ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/grpc/migration?view=aspnetcore-3.0)
