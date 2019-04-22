---
title: 'Esercitazione: Creare un client gRPC .NET Core'
author: juntaoluo
description: Questa serie di esercitazioni illustra come creare un servizio gRPC in ASP.NET Core. Informazioni su come creare un progetto di servizio gRPC, modificare un file con estensione proto e aggiungere una chiamata con flusso bidirezionale.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 4/10/2019
uid: tutorials/grpc/grpc-client
ms.openlocfilehash: 031afbfaf097c518a85400b0b6abbc135c1bc611
ms.sourcegitcommit: 57a974556acd09363a58f38c26f74dc21e0d4339
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59674142"
---
# <a name="tutorial-create-a-net-core-grpc-client"></a>Esercitazione: Creare un client gRPC .NET Core

A cura di [John Luo](https://github.com/juntaoluo)

Questa esercitazione illustra come creare un client [gRPC](https://grpc.io/docs/guides/) .NET Core in grado di comunicare con i servizi gRPC.

Al termine, si otterrà un client gRPC che comunica con il servizio Greeter gRPC.

[!INCLUDE[View or download sample code](~/includes/grpc/downloadClient.md)]

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Creare un client gRPC.
> * Eseguire il client sul servizio gRPC Greeter creato nell'esercitazione precedente.
> * Esaminare i file di progetto.

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-net-console-application"></a>Creare un'applicazione console .NET

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Seguire le istruzioni riportate [qui](https://docs.microsoft.com/en-us/dotnet/core/tutorials/with-visual-studio) per creare un'app console con il nome *GrpcGreeterClient*.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Seguire le istruzioni riportate [qui](https://docs.microsoft.com/en-us/dotnet/core/tutorials/with-visual-studio-code) per creare un'app console con il nome *GrpcGreeterClient*.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

Seguire le istruzioni riportate [qui](https://docs.microsoft.com/en-us/dotnet/core/tutorials/using-on-mac-vs-full-solution) per creare un'app console con il nome *GrpcGreeterClient*.

<!-- End of VS tabs -->

---

## <a name="add-required-packages"></a>Aggiungere i pacchetti necessari

Aggiungere i pacchetti seguenti nel progetto client gRPC:

* [Grpc.Core](https://www.nuget.org/packages/Grpc.Core), che contiene l'API C# per il client C-core.
* [Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), che contiene le API per i messaggi protobuf per C#.
* [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), che contiene il supporto di strumenti C# per i file protobuf. Il pacchetto di strumenti non è necessario in fase di esecuzione, quindi la dipendenza è contrassegnata come `PrivateAssets="All"`.

È possibile aggiungere i pacchetti con gli approcci seguenti:

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="pmc-option-to-install-packages"></a>Opzione con la console di Gestione pacchetti per installare i pacchetti

* Da Visual Studio selezionare **Strumenti** > **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**
* Dalla finestra **Console di Gestione pacchetti** passare alla directory in cui si trova il file *GrpcGreeterClient.csproj*.
* Eseguire il comando seguente:

    ```powershell
    Install-Package Grpc.Core
    ```

* Ripetere `Install-Package` per Google.Protobuf e Grpc.Tools

<!-- Tutorials shouldn't have multiple options. Select what you think is the best approach. Recommend you removed this approach. -->

### <a name="manage-nuget-packages-option-to-install-packages"></a>Opzione Gestisci pacchetti NuGet per installare i pacchetti

* Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** > **Gestisci pacchetti NuGet**
* Impostare **Origine pacchetto** su "nuget.org"
* Immettere "Grpc.Core" nella casella di ricerca
* Selezionare il pacchetto ".NET Core" dalla scheda **Sfoglia** e fare clic su **Installa**
* Ripetere per Google.Protobuf e Grpc.Tools

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Eseguire il comando seguente da **Terminale integrato**:

```console
dotnet add TodoApi.csproj package Grpc.Core
```

Ripetere per Google.Protobuf e Grpc.Tools

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

* Fare clic su con il pulsante destro del mouse sulla cartella *Pacchetti* in **Solution Pad** (Riquadro della soluzione) > **Aggiungi pacchetti**
* Impostare l'elenco a discesa **Aggiungi pacchetti** della finestra **Origine** su "nuget.org"
* Immettere "Grpc.Core" nella casella di ricerca
* Selezionare il pacchetto "Grpc.Core" nel riquadro dei risultati e fare clic su **Aggiungi pacchetto**
* Ripetere per Google.Protobuf e Grpc.Tools

---

## <a name="add-the-greetproto-file"></a>Aggiungere il file greet.proto

Copiare il file **Protos\greet.proto** dal servizio Greeter gRPC al progetto client gRPC. Aggiungere il file **greet.proto** al gruppo di elementi `<Protobuf>` del file di progetto GrpcGreeterClient:

```XML
<Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
```

> [!NOTE]
> Per aprire il file di progetto di GrpcGreeterClient, fare clic con il pulsante destro del mouse sul progetto e scegliere **Modifica GrpcGreeterClient.csproj** dal menu a discesa.
>
> ![nuova applicazione Web ASP.NET Core](grpc-start/_static/edit_csproj.png)
>
> In alternativa, è possibile passare alla directory GrpcGreeterClient e modificare il file `GrpcGreeterClient.csproj` con l'editor preferito.

L'attributo `GrpcServices="Client"` viene aggiunto in modo che vengano generati solo gli asset client C# per il file protobuf incluso. Compilare il progetto client per attivare la generazione degli asset client C#.

## <a name="create-a-greeterclient-and-invoke-the-sayhello-unary-call"></a>Creare un GreeterClient e richiamare la chiamata unaria SayHello

Aggiungere il codice seguente al metodo `Main` del file `Program.cs` del progetto client gRPC:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet)]

Per accedere ai tipi richiesti, sono necessarie le istruzioni using seguenti:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=using)]

GreeterClient viene creato creando un'istanza di `Channel` contenente le informazioni per la creazione della connessione al servizio gRPC e usandolo per costruire `GreeterClient`:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=4-5)]

GreeterClient contiene la chiamata unaria `SayHello` che può essere richiamata in modo asincrono:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-8)]

I risultati della chiamata `SayHello` vengono archiviati in `reply` e potranno poi essere visualizzati:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=9)]

Il `Channel` usato dal client deve essere arrestato al termine delle operazioni per rilasciare tutte le risorse:

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?name=snippet&highlight=11)]

> [!NOTE]
> È necessario compilare il progetto prima di poter risolvere i tipi nello spazio dei nomi **Greeter**. Questi tipi vengono generati automaticamente durante la compilazione e non sono disponibili prima dell'esecuzione di una compilazione.

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a>Testare il client gRPC con il servizio Greeter gRPC

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Verificare che il servizio Greeter creato nell'esercitazione precedente sia in esecuzione.

* Quando il servizio è in esecuzione, tornare al progetto **GrpcGreeterClient** e impostarlo come progetto di avvio. Premere CTRL+F5 per eseguire il client senza il debugger.

  Il client invia un saluto al servizio con un messaggio contenente il nome "GreeterClient". Il servizio invierà un messaggio "Hello GreeterClient" come risposta visualizzata al prompt dei comandi.

  ![nuova applicazione Web ASP.NET Core](grpc-start/_static/client.png)

  Il servizio registra i dettagli della chiamata con esito positivo nei log scritti al prompt dei comandi.

  ![nuova applicazione Web ASP.NET Core](grpc-start/_static/server_complete.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio per Mac](#tab/visual-studio-code+visual-studio-mac)

* Verificare che il servizio Greeter creato nell'esercitazione precedente sia in esecuzione.

* Eseguire il progetto client GrpcGreeter.Client dalla riga di comando separata tramite `dotnet run`.

Il client invia un saluto al servizio con un messaggio contenente il nome "GreeterClient". Il servizio invierà un messaggio "Hello GreeterClient" come risposta visualizzata al prompt dei comandi.

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

Il servizio registra i dettagli della chiamata con esito positivo nei log scritti al prompt dei comandi.

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

### <a name="examine-the-project-files-of-the-grpc-project"></a>Esaminare i file di progetto del progetto gRPC

File GrpcGreeterClient del client gRPC:

*Program.cs* contiene la logica e il punto di ingresso per il client gRPC.

## <a name="additional-resources"></a>Risorse aggiuntive

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Creare un client gRPC.
> * Eseguire il client sul servizio gRPC Greeter creato nell'esercitazione precedente.
> * Esaminare i file di progetto.

> [!div class="step-by-step"]
> [Precedente: Creare un servizio Greeter gRPC](xref:tutorials/grpc/grpc-start)
