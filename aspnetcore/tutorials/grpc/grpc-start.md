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
# <a name="tutorial-get-started-with-grpc-service-in-aspnet-core"></a>Esercitazione: Introduzione all'uso del servizio gRPC in ASP.NET Core

A cura di [John Luo](https://github.com/juntaoluo)

Questa esercitazione illustra le nozioni di base della creazione di un servizio gRPC in ASP.NET Core.

Al termine dell'esercitazione si avrà un servizio gRPC che restituisce messaggi di apertura.

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Creare un servizio gRPC.
> * Eseguire il servizio.
> * Esaminare i file di progetto.

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a>Creare un servizio gRPC

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Dal menu **File** di Visual Studio selezionare **Nuovo** > **Progetto**.
* Creare una nuova applicazione Web ASP.NET Core.
  ![nuova applicazione Web ASP.NET Core](grpc-start/_static/np_3_0.1.png)
* Denominare il progetto **GrpcGreeter**. È importante denominare il progetto *GrpcGreeter* in modo che gli spazi dei nomi corrispondano nell'operazione copia/incolla del codice.
  ![nuova applicazione Web ASP.NET Core](grpc-start/_static/np_3_0.2.png)
* Selezionare **.NET Core** e **ASP.NET Core 3.0** nell'elenco a discesa. Scegliere il modello del **servizio gRPC**.

  Viene creato il progetto iniziale seguente:

  ![Esplora soluzioni](grpc-start/_static/se3.0.png)

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

### <a name="test-the-service"></a>Eseguire il test del servizio

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Verificare che il **GrpcGreeter.Server** sia impostato come progetto di avvio e premere CTRL+F5 per eseguire il servizio gRPC senza il debugger.

  Visual Studio esegue il servizio in un prompt dei comandi. I log mostrano che il servizio ha avviato l'ascolto su `http://localhost:50051`.

  ![nuova applicazione Web ASP.NET Core](grpc-start/_static/server_start.png)

* Quando il servizio è in esecuzione, verificare che il **GrpcGreeter.Client** sia impostato come progetto di avvio e premere CTRL+F5 per eseguire il client senza il debugger.

  Il client invia un saluto al servizio con un messaggio contenente il nome "GreeterClient". Il servizio invierà un messaggio "Hello GreeterClient" come risposta visualizzata al prompt dei comandi.

  ![nuova applicazione Web ASP.NET Core](grpc-start/_static/client.png)

  Il servizio registra i dettagli della chiamata con esito positivo nei log scritti al prompt dei comandi.

  ![nuova applicazione Web ASP.NET Core](grpc-start/_static/server_complete.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio per Mac](#tab/visual-studio-code+visual-studio-mac)

* Eseguire il progetto server GrpcGreeter.Server dalla riga di comando tramite `dotnet run`. I log mostrano che il servizio ha avviato l'ascolto su `http://localhost:50051`.

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

* Eseguire il progetto client GrpcGreeter.Client dalla riga di comando separata tramite `dotnet run`.

Il client invia un saluto al servizio con un messaggio contenente il nome "GreeterClient". Il servizio invierà un messaggio "Hello GreeterClient" come risposta visualizzata al prompt dei comandi.

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

Il servizio registra i dettagli della chiamata con esito positivo nei log scritti al prompt dei comandi.

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

### <a name="examine-the-project-files-of-the-grpc-project"></a>Esaminare i file di progetto del progetto gRPC

File GrpcGreeter.Server:

* greet.proto: Il file *Protos/greet.proto* definisce il gRPC `Greeter` e viene usato per generare le risorse del server gRPC. Per ulteriori informazioni, vedere <xref:grpc/index>.
* Cartella *Servizi*: contiene l'implementazione del servizio `Greeter`.
* *appSettings. JSON*: contiene i dati di configurazione, ad esempio il protocollo usato da Kestrel. Per ulteriori informazioni, vedere <xref:fundamentals/configuration/index>.
* *Program.cs*: Contiene il punto di ingresso per il servizio gRPC. Per ulteriori informazioni, vedere <xref:fundamentals/host/web-host>.
* Startup.cs

Contiene codice che consente di configurare il comportamento dell'app. Per ulteriori informazioni, vedere <xref:fundamentals/startup>.

File GrpcGreeter.Client del client gRPC:

*Program.cs* contiene la logica e il punto di ingresso per il client gRPC.

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * È stato creato un servizio gRPC.
> * Sono stati eseguiti il servizio e un client per testare il servizio.
> * Esame dei file di progetto.
