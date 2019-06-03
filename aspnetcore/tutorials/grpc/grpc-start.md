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
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a>Esercitazione: Creare un client e un server gRPC in ASP.NET Core

A cura di [John Luo](https://github.com/juntaoluo)

Questa esercitazione illustra come creare un client [gRPC](https://grpc.io/docs/guides/) .NET Core e un server gRPC ASP.NET Core.

Al termine, si otterrà un client gRPC che comunica con il servizio Greeter gRPC.

[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([procedura per il download](xref:index#how-to-download-a-sample)).

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Creare un server gRPC.
> * Creare un client gRPC.
> * Testare il servizio client gRPC con il servizio Greeter gRPC.

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a>Creare un servizio gRPC

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.
* Nella finestra di dialogo **Crea un nuovo progetto** selezionare **Applicazione Web ASP.NET Core**.
* Scegliere **Avanti**
* Denominare il progetto **GrpcGreeter**. È importante denominare il progetto *GrpcGreeter* in modo che gli spazi dei nomi corrispondano nell'operazione copia/incolla del codice.
* Selezionare **Crea**.
* Nella finestra di dialogo **Crea una nuova applicazione Web ASP.NET Core**:
  * Selezionare **.NET Core** e **ASP.NET Core 3.0** negli elenchi a discesa. 
  * Selezionare il modello del **servizio gRPC**.
  * Selezionare **Crea**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Cambiare directory (`cd`) e passare alla cartella che conterrà il progetto.
* Eseguire i comandi seguenti:

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * Il comando `dotnet new` crea un nuovo servizio gRPC nella cartella *GrpcGreeter*.
  * Il comando `code` apre la cartella *GrpcGreeter* in una nuova istanza di Visual Studio Code.

  Viene visualizzata una finestra di dialogo con **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?** (Risorse di compilazione e debug mancanti da "RazorPagesMovie". Aggiungerle?)
* Selezionare **Sì**.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

Da un terminale eseguire i comandi seguenti:

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

I comandi precedenti usano l'[interfaccia della riga di comando di .NET Core](/dotnet/core/tools/dotnet) per creare un servizio gRPC.

### <a name="open-the-project"></a>Aprire il progetto

In Visual Studio selezionare **File > Apri** e quindi selezionare il file *GrpcGreeter.sln*.

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a>Eseguire il servizio

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Premere CTRL+F5 per eseguire il servizio gRPC senza il debugger.

  Visual Studio esegue il servizio in un prompt dei comandi.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio per Mac](#tab/visual-studio-code+visual-studio-mac)

* Eseguire il progetto gRPC Greeter GrpcGreeter dalla riga di comando tramite `dotnet run`.

<!-- End of combined VS/Mac tabs -->

---

I log indicano che il servizio è in ascolto su `http://localhost:50051`.

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

### <a name="examine-the-project-files"></a>Esaminare i file di progetto

File GrpcGreeter:

* *greet.proto*: Il file *Protos/greet.proto* definisce il gRPC `Greeter` e viene usato per generare le risorse del server gRPC. Per altre informazioni, vedere [Introduzione a gRPC](xref:grpc/index).
* Cartella *Servizi*: contiene l'implementazione del servizio `Greeter`.
* *appSettings.json*: contiene i dati di configurazione, ad esempio il protocollo usato da Kestrel. Per ulteriori informazioni, vedere <xref:fundamentals/configuration/index>.
* *Program.cs*: Contiene il punto di ingresso per il servizio gRPC. Per ulteriori informazioni, vedere <xref:fundamentals/host/web-host>.
* *Startup.cs*: Contiene codice che consente di configurare il comportamento dell'app. Per altre informazioni, vedere [Avvio dell'app](xref:fundamentals/startup).

## <a name="create-the-grpc-client-in-a-net-console-app"></a>Creare il client gRPC in un'app console .NET

## <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Selezionare **File** > **Nuovo** > **Progetto** dalla barra dei menu.
* Nella finestra di dialogo **Crea un nuovo progetto** selezionare **App console (.NET Core)** .
* Scegliere **Avanti**
* Nella casella di testo **Nome** immettere "GrpcGreeterClient".
* Scegliere **Crea**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Cambiare directory (`cd`) e passare alla cartella che conterrà il progetto.
* Eseguire i comandi seguenti:

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

Seguire le istruzioni riportate [qui](/dotnet/core/tutorials/using-on-mac-vs-full-solution) per creare un'app console con il nome *GrpcGreeterClient*.

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a>Aggiungere i pacchetti necessari

Aggiungere i pacchetti seguenti nel progetto client gRPC:

* [Grpc.Core](https://www.nuget.org/packages/Grpc.Core), che contiene l'API C# per il client C-core.
* [Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), che contiene le API per i messaggi protobuf per C#.
* [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), che contiene il supporto di strumenti C# per i file protobuf. Il pacchetto di strumenti non è necessario in fase di esecuzione, quindi la dipendenza è contrassegnata come `PrivateAssets="All"`.

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Installare i pacchetti tramite la Console di gestione pacchetti (PMC) o Gestisci pacchetti NuGet

####  <a name="pmc-option-to-install-packages"></a>Opzione con la console di Gestione pacchetti per installare i pacchetti

* Da Visual Studio selezionare **Strumenti** > **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**
* Dalla finestra **Console di Gestione pacchetti** passare alla directory in cui si trova il file *GrpcGreeterClient.csproj*.
* Eseguire i comandi seguenti:

 ```powershell
Install-Package Grpc.Core
Install-Package Grpc.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a>Opzione Gestisci pacchetti NuGet per installare i pacchetti

* Fare clic con il pulsante destro del mouse sul progetto in **Esplora soluzioni** > **Gestisci pacchetti NuGet**
* Selezionare la scheda **Sfoglia**.
* Immettere **Grpc.Core** nella casella di ricerca.
* Selezionare il pacchetto **Grpc.Core** dalla scheda **Sfoglia** e fare clic su **Installa**.
* Ripetere la procedura per `Google.Protobuf` e `Grpc.Tools`.

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Eseguire i comandi seguenti dal **terminale integrato**:

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Core
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

* Fare clic con il pulsante destro del mouse sulla cartella **Pacchetti** nel **riquadro della soluzione** > **Aggiungi pacchetti**
* Immettere **Grpc.Core** nella casella di ricerca.
* Selezionare il pacchetto **Grpc.Core** nel riquadro dei risultati e fare clic su **Aggiungi pacchetto**
* Ripetere la procedura per `Google.Protobuf` e `Grpc.Tools`.

---

### <a name="add-greetproto"></a>Aggiungere greet.proto

* Creare una cartella **Protos** nel progetto client gRPC.
* Copiare il file **Protos\greet.proto** dal servizio Greeter gRPC al progetto client gRPC.
* Modificare il file di progetto *GrpcGreeterClient.csproj*:

  # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

  Fare clic con il pulsante destro del mouse sul progetto e selezionare **Modifica GrpcGreeterClient.csproj**.

  # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code) 

  Selezionare il file *GrpcGreeterClient.csproj*.

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

  Fare clic con il pulsante destro del mouse sul progetto e selezionare **Strumenti > Modifica File**.

  ------

* Aggiungere il file **greet.proto** al gruppo di elementi `<Protobuf>` del file di progetto GrpcGreeterClient:

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

Compilare il progetto client per attivare la generazione degli asset client C#.

### <a name="create-the-greater-client"></a>Creare il client Greeter

Compilare il progetto per creare i tipi nello spazio dei nomi **Greeter**. I tipi `Greeter` vengono generati automaticamente dal processo di compilazione.

Aggiornare il file *Program.cs* del client gRPC con il codice seguente:

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

*Program.cs* contiene la logica e il punto di ingresso per il client gRPC.

Il client Greeter viene creato come descritto di seguito:

* Creare un'istanza di `Channel` contenente le informazioni per creare la connessione al servizio gRPC.
* Usare `Channel` per costruire il client Greeter:

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=4-6)]

Il client Greeter chiama il metodo `SayHello` asincrono. Viene visualizzato il risultato della chiamata `SayHello`:

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-9)]

Arrestare il `Channel` usato dal client al termine delle operazioni per rilasciare tutte le risorse.

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a>Testare il client gRPC con il servizio Greeter gRPC

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Nel servizio Greeter premere CTRL+F5 per avviare il server senza il debugger.
* Nel progetto GrpcGreeterClient premere CTRL+F5 per avviare il server senza il debugger.

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio per Mac](#tab/visual-studio-code+visual-studio-mac)

* Avviare il servizio Greeter.
* Avviare il client.

<!-- End of combined VS/Mac tabs -->

---

Il client invia un saluto al servizio con un messaggio contenente il nome "GreeterClient". Il servizio invia il messaggio "Hello GreeterClient" come risposta. La risposta "Hello GreeterClient" viene visualizzata al prompt dei comandi:

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

Il servizio gRPC registra i dettagli della chiamata con esito positivo nei log scritti al prompt dei comandi.

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

### <a name="next-steps"></a>Passaggi successivi

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
