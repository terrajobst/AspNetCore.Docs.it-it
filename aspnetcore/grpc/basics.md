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
# <a name="grpc-services-with-c"></a><span data-ttu-id="ea683-103">servizi gRPC con C\#</span><span class="sxs-lookup"><span data-stu-id="ea683-103">gRPC services with C\#</span></span>

<span data-ttu-id="ea683-104">Questo documento sono illustrati i concetti necessari per scrivere [gRPC](https://grpc.io/docs/guides/) app C#.</span><span class="sxs-lookup"><span data-stu-id="ea683-104">This document outlines the concepts needed to write [gRPC](https://grpc.io/docs/guides/) apps in C#.</span></span> <span data-ttu-id="ea683-105">Gli argomenti descritti in questo argomento si applicano a entrambe [C-core](https://grpc.io/blog/grpc-stacks)-App basate su ASP.NET Core e basata su gRPC.</span><span class="sxs-lookup"><span data-stu-id="ea683-105">The topics covered here apply to both [C-core](https://grpc.io/blog/grpc-stacks)-based and ASP.NET Core-based gRPC apps.</span></span>

## <a name="proto-file"></a><span data-ttu-id="ea683-106">file di prototipo</span><span class="sxs-lookup"><span data-stu-id="ea683-106">proto file</span></span>

<span data-ttu-id="ea683-107">gRPC Usa un approccio incentrato contratto per lo sviluppo di API.</span><span class="sxs-lookup"><span data-stu-id="ea683-107">gRPC uses a contract-first approach to API development.</span></span> <span data-ttu-id="ea683-108">I buffer di protocollo (protobuf) vengono utilizzati come linguaggio di progettazione di interfaccia (IDL) per impostazione predefinita.</span><span class="sxs-lookup"><span data-stu-id="ea683-108">Protocol buffers (protobuf) are used as the Interface Design Language (IDL) by default.</span></span> <span data-ttu-id="ea683-109">Il *.proto* contiene file:</span><span class="sxs-lookup"><span data-stu-id="ea683-109">The *.proto* file contains:</span></span>

* <span data-ttu-id="ea683-110">La definizione del servizio gRPC.</span><span class="sxs-lookup"><span data-stu-id="ea683-110">The definition of the gRPC service.</span></span>
* <span data-ttu-id="ea683-111">I messaggi inviati tra client e server.</span><span class="sxs-lookup"><span data-stu-id="ea683-111">The messages sent between clients and servers.</span></span>

<span data-ttu-id="ea683-112">Per altre informazioni sulla sintassi dei file protobuf, vedere la [ufficiali (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).</span><span class="sxs-lookup"><span data-stu-id="ea683-112">For more information on the syntax of protobuf files, see the [official documentation (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).</span></span>

<span data-ttu-id="ea683-113">Si consideri, ad esempio, il *greet.proto* file utilizzato nelle [iniziare a usare servizio gRPC](xref:tutorials/grpc/grpc-start):</span><span class="sxs-lookup"><span data-stu-id="ea683-113">For example, consider the *greet.proto* file used in [Get started with gRPC service](xref:tutorials/grpc/grpc-start):</span></span>

* <span data-ttu-id="ea683-114">Definisce un `Greeter` servizio.</span><span class="sxs-lookup"><span data-stu-id="ea683-114">Defines a `Greeter` service.</span></span>
* <span data-ttu-id="ea683-115">Il `Greeter` servizio definisce un `SayHello` chiamare.</span><span class="sxs-lookup"><span data-stu-id="ea683-115">The `Greeter` service defines a `SayHello` call.</span></span>
* <span data-ttu-id="ea683-116">`SayHello` Invia un `HelloRequest` dei messaggi e riceve un `HelloResponse` messaggio:</span><span class="sxs-lookup"><span data-stu-id="ea683-116">`SayHello` sends a `HelloRequest` message and receives a `HelloResponse` message:</span></span>

[!code-proto[](~/tutorials/grpc/grpc-start/samples/GrpcStart/Protos/greet.proto)]

## <a name="add-a-proto-file-to-a-c-app"></a><span data-ttu-id="ea683-117">Aggiungere un file .proto a C\# app</span><span class="sxs-lookup"><span data-stu-id="ea683-117">Add a .proto file to a C\# app</span></span>

<span data-ttu-id="ea683-118">Il *.proto* file è incluso in un progetto, aggiungerlo al `<Protobuf>` gruppo di elementi:</span><span class="sxs-lookup"><span data-stu-id="ea683-118">The *.proto* file is included in a project by adding it to the `<Protobuf>` item group:</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/GrpcGreeter.Server.csproj?highlight=2&range=7-10)]

## <a name="c-tooling-support-for-proto-files"></a><span data-ttu-id="ea683-119">C#Supporto per i file .proto degli strumenti</span><span class="sxs-lookup"><span data-stu-id="ea683-119">C# Tooling support for .proto files</span></span>

<span data-ttu-id="ea683-120">Il pacchetto degli strumenti [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) necessarie per generare il C# asset dai *.proto* file.</span><span class="sxs-lookup"><span data-stu-id="ea683-120">The tooling package [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) is required to generate the C# assets from *.proto* files.</span></span> <span data-ttu-id="ea683-121">Degli asset generati (file):</span><span class="sxs-lookup"><span data-stu-id="ea683-121">The generated assets (files):</span></span>

* <span data-ttu-id="ea683-122">Vengono generati ogni volta che il progetto viene compilato in base alle esigenze.</span><span class="sxs-lookup"><span data-stu-id="ea683-122">Are generated on an as-needed basis each time the project is built.</span></span>
* <span data-ttu-id="ea683-123">Non sono aggiunti al progetto o archiviato nel controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="ea683-123">Aren't added to the project or checked into source control.</span></span>
* <span data-ttu-id="ea683-124">Sono contenuto in un artefatto di compilazione il *obj* directory.</span><span class="sxs-lookup"><span data-stu-id="ea683-124">Are a build artifact contained in the *obj* directory.</span></span>

<span data-ttu-id="ea683-125">Questo pacchetto è obbligatoria per i progetti server e client.</span><span class="sxs-lookup"><span data-stu-id="ea683-125">This package is required by both the server and client projects.</span></span> <span data-ttu-id="ea683-126">`Grpc.Tools` possono essere aggiunti con la gestione dei pacchetti in Visual Studio o aggiungendo un `<PackageReference>` al file di progetto:</span><span class="sxs-lookup"><span data-stu-id="ea683-126">`Grpc.Tools` can be added by using the Package Manager in Visual Studio or adding a `<PackageReference>` to the project file:</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/GrpcGreeter.Server.csproj?highlight=1&range=16)]

<span data-ttu-id="ea683-127">Il pacchetto di strumenti non è necessario in fase di esecuzione, in modo che la dipendenza è contrassegnata con `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="ea683-127">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

## <a name="generated-c-assets"></a><span data-ttu-id="ea683-128">Generato C# Asset</span><span class="sxs-lookup"><span data-stu-id="ea683-128">Generated C# assets</span></span>

<span data-ttu-id="ea683-129">Genera il pacchetto di strumenti di C# tipi che rappresentano i messaggi definiti in inclusi *.proto* i file.</span><span class="sxs-lookup"><span data-stu-id="ea683-129">The tooling package generates the C# types representing the messages defined in the included *.proto* files.</span></span>

<span data-ttu-id="ea683-130">Per gli asset sul lato server, viene generato un tipo di base astratta del servizio.</span><span class="sxs-lookup"><span data-stu-id="ea683-130">For server-side assets, an abstract service base type is generated.</span></span> <span data-ttu-id="ea683-131">Il tipo di base contiene le definizioni di tutte le chiamate gRPC contenute nel *.proto* file.</span><span class="sxs-lookup"><span data-stu-id="ea683-131">The base type contains the definitions of all the gRPC calls contained in the *.proto* file.</span></span> <span data-ttu-id="ea683-132">Creare un'implementazione concreta di servizio che deriva da questo tipo di base e implementa la logica per le chiamate gRPC.</span><span class="sxs-lookup"><span data-stu-id="ea683-132">Create a concrete service implementation that derives from this base type and implements the logic for the gRPC calls.</span></span> <span data-ttu-id="ea683-133">Per il `greet.proto`, l'esempio descritto in precedenza, una classe astratta `GreeterBase` tipo contenente una virtuale `SayHello` metodo viene generato.</span><span class="sxs-lookup"><span data-stu-id="ea683-133">For the `greet.proto`, the example described previously, an abstract `GreeterBase` type that contains a virtual `SayHello` method is generated.</span></span> <span data-ttu-id="ea683-134">Un'implementazione concreta `GreeterService` esegue l'override del metodo e implementa la logica che gestisce la chiamata gRPC.</span><span class="sxs-lookup"><span data-stu-id="ea683-134">A concrete implementation `GreeterService` overrides the method and implements the logic handling the gRPC call.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/Services/GreeterService.cs?name=snippet)]

<span data-ttu-id="ea683-135">Per le risorse lato client, viene generato un tipo concreto di client.</span><span class="sxs-lookup"><span data-stu-id="ea683-135">For client-side assets, a concrete client type is generated.</span></span> <span data-ttu-id="ea683-136">Chiama il gRPC il *.proto* file vengono convertite in metodi per il tipo concreto, che può essere chiamato.</span><span class="sxs-lookup"><span data-stu-id="ea683-136">The gRPC calls in the *.proto* file are translated into methods on the concrete type, which can be called.</span></span> <span data-ttu-id="ea683-137">Per il `greet.proto`, l'esempio descritto in precedenza, un concreto `GreeterClient` tipo viene generato.</span><span class="sxs-lookup"><span data-stu-id="ea683-137">For the `greet.proto`, the example described previously, a concrete `GreeterClient` type is generated.</span></span> <span data-ttu-id="ea683-138">Chiamare `GreeterClient.SayHello` per avviare una chiamata gRPC al server.</span><span class="sxs-lookup"><span data-stu-id="ea683-138">Call `GreeterClient.SayHello` to initiate a gRPC call to the server.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Client/Program.cs?highlight=9-11&name=snippet)]

<span data-ttu-id="ea683-139">Per impostazione predefinita, gli asset di server e client vengono generati per ogni *.proto* incluso nel file di `<Protobuf>` gruppo di elementi.</span><span class="sxs-lookup"><span data-stu-id="ea683-139">By default, server and client assets are generated for each *.proto* file included in the `<Protobuf>` item group.</span></span> <span data-ttu-id="ea683-140">Per assicurarsi che solo le risorse server vengono generate in un progetto server, il `GrpcServices` attributo è impostato su `Server`.</span><span class="sxs-lookup"><span data-stu-id="ea683-140">To ensure only the server assets are generated in a server project, the `GrpcServices` attribute is set to `Server`.</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/GrpcGreeter.Server.csproj?highlight=2&range=7-10)]

<span data-ttu-id="ea683-141">Analogamente, l'attributo è impostato su `Client` nei progetti client.</span><span class="sxs-lookup"><span data-stu-id="ea683-141">Similarly, the attribute is set to `Client` in client projects.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ea683-142">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="ea683-142">Additional resources</span></span>

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>
