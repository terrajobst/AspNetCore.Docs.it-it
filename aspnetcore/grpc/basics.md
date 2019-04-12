---
title: Servizi gRPC con C#
author: juntaoluo
description: Informazioni sui concetti di base durante la scrittura di servizi gRPC con C#.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/basics
ms.openlocfilehash: ce2682848dc6a81293545c27f0be779e12a3a600
ms.sourcegitcommit: 5995f44e9e13d7e7aa8d193e2825381c42184e47
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/02/2019
ms.locfileid: "59515515"
---
# <a name="grpc-services-with-c"></a>servizi gRPC con C\#

Questo documento sono illustrati i concetti necessari per scrivere [gRPC](https://grpc.io/docs/guides/) app C#. Gli argomenti descritti in questo argomento si applicano a entrambe [C-core](https://grpc.io/blog/grpc-stacks)-App basate su ASP.NET Core e basata su gRPC.

## <a name="proto-file"></a>file di prototipo

gRPC Usa un approccio incentrato contratto per lo sviluppo di API. I buffer di protocollo (protobuf) vengono utilizzati come linguaggio di progettazione di interfaccia (IDL) per impostazione predefinita. Il *.proto* contiene file:

* La definizione del servizio gRPC.
* I messaggi inviati tra client e server.

Per altre informazioni sulla sintassi dei file protobuf, vedere la [ufficiali (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).

Si consideri, ad esempio, il *greet.proto* file utilizzato nelle [iniziare a usare servizio gRPC](xref:tutorials/grpc/grpc-start):

* Definisce un `Greeter` servizio.
* Il `Greeter` servizio definisce un `SayHello` chiamare.
* `SayHello` Invia un `HelloRequest` dei messaggi e riceve un `HelloResponse` messaggio:

[!code-proto[](~/tutorials/grpc/grpc-start/samples/GrpcStart/Protos/greet.proto)]

## <a name="add-a-proto-file-to-a-c-app"></a>Aggiungere un file .proto a C\# app

Il *.proto* file è incluso in un progetto, aggiungerlo al `<Protobuf>` gruppo di elementi:

[!code-xml[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/GrpcGreeter.Server.csproj?highlight=2&range=7-10)]

## <a name="c-tooling-support-for-proto-files"></a>C#Supporto per i file .proto degli strumenti

Il pacchetto degli strumenti [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) necessarie per generare il C# asset dai *.proto* file. Degli asset generati (file):

* Vengono generati ogni volta che il progetto viene compilato in base alle esigenze.
* Non sono aggiunti al progetto o archiviato nel controllo del codice sorgente.
* Sono contenuto in un artefatto di compilazione il *obj* directory.

Questo pacchetto è obbligatoria per i progetti server e client. `Grpc.Tools` possono essere aggiunti con la gestione dei pacchetti in Visual Studio o aggiungendo un `<PackageReference>` al file di progetto:

[!code-xml[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/GrpcGreeter.Server.csproj?highlight=1&range=16)]

Il pacchetto di strumenti non è necessario in fase di esecuzione, in modo che la dipendenza è contrassegnata con `PrivateAssets="All"`.

## <a name="generated-c-assets"></a>Generato C# Asset

Genera il pacchetto di strumenti di C# tipi che rappresentano i messaggi definiti in inclusi *.proto* i file.

Per gli asset sul lato server, viene generato un tipo di base astratta del servizio. Il tipo di base contiene le definizioni di tutte le chiamate gRPC contenute nel *.proto* file. Creare un'implementazione concreta di servizio che deriva da questo tipo di base e implementa la logica per le chiamate gRPC. Per il `greet.proto`, l'esempio descritto in precedenza, una classe astratta `GreeterBase` tipo contenente una virtuale `SayHello` metodo viene generato. Un'implementazione concreta `GreeterService` esegue l'override del metodo e implementa la logica che gestisce la chiamata gRPC.

[!code-csharp[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/Services/GreeterService.cs?name=snippet)]

Per le risorse lato client, viene generato un tipo concreto di client. Chiama il gRPC il *.proto* file vengono convertite in metodi per il tipo concreto, che può essere chiamato. Per il `greet.proto`, l'esempio descritto in precedenza, un concreto `GreeterClient` tipo viene generato. Chiamare `GreeterClient.SayHello` per avviare una chiamata gRPC al server.

[!code-csharp[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Client/Program.cs?highlight=9-11&name=snippet)]

Per impostazione predefinita, gli asset di server e client vengono generati per ogni *.proto* incluso nel file di `<Protobuf>` gruppo di elementi. Per assicurarsi che solo le risorse server vengono generate in un progetto server, il `GrpcServices` attributo è impostato su `Server`.

[!code-xml[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/GrpcGreeter.Server.csproj?highlight=2&range=7-10)]

Analogamente, l'attributo è impostato su `Client` nei progetti client.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>
