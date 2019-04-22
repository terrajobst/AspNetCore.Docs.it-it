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
# <a name="tutorial-get-started-with-grpc-service-in-aspnet-core"></a>Esercitazione: Introduzione all'uso del servizio gRPC in ASP.NET Core

A cura di [John Luo](https://github.com/juntaoluo)

Questa esercitazione illustra le nozioni di base della creazione di un servizio gRPC in ASP.NET Core.

Al termine dell'esercitazione si avrà un servizio gRPC che restituisce messaggi di apertura.

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Creare un servizio gRPC.
> * Eseguire il servizio gRPC.
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

### <a name="run-the-service"></a>Eseguire il servizio

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Premere CTRL+F5 per eseguire il servizio gRPC senza il debugger.

  Visual Studio esegue il servizio in un prompt dei comandi. I log mostrano che il servizio ha avviato l'ascolto su `http://localhost:50051`.

  ![nuova applicazione Web ASP.NET Core](grpc-start/_static/server_start.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio per Mac](#tab/visual-studio-code+visual-studio-mac)

* Eseguire il progetto gRPC Greeter GrpcGreeter dalla riga di comando tramite `dotnet run`. I log mostrano che il servizio ha avviato l'ascolto su `http://localhost:50051`.

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

L'esercitazione successiva illustra come creare un client gRPC, necessario per testare il servizio Greeter.

### <a name="examine-the-project-files-of-the-grpc-project"></a>Esaminare i file di progetto del progetto gRPC

File GrpcGreeter:

* greet.proto: Il file *Protos/greet.proto* definisce il gRPC `Greeter` e viene usato per generare le risorse del server gRPC. Per ulteriori informazioni, vedere <xref:grpc/index>.
* Cartella *Servizi*: contiene l'implementazione del servizio `Greeter`.
* *appSettings. JSON*: contiene i dati di configurazione, ad esempio il protocollo usato da Kestrel. Per ulteriori informazioni, vedere <xref:fundamentals/configuration/index>.
* *Program.cs*: Contiene il punto di ingresso per il servizio gRPC. Per ulteriori informazioni, vedere <xref:fundamentals/host/web-host>.
* Startup.cs: Contiene codice che consente di configurare il comportamento dell'app. Per ulteriori informazioni, vedere <xref:fundamentals/startup>.

### <a name="test-the-service"></a>Eseguire il test del servizio

## <a name="additional-resources"></a>Risorse aggiuntive

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * È stato creato un servizio gRPC.
> * È stato eseguito il servizio gRPC.
> * Esame dei file di progetto.

> [!div class="step-by-step"]
> [Successivo: Creare un client gRPC .NET Core](xref:tutorials/grpc/grpc-client)