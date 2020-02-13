---
title: Usare gRPC nelle app del browser
author: jamesnk
description: Informazioni su come configurare i servizi gRPC in ASP.NET Core per essere richiamabili dalle app del browser usando gRPC-Web.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 02/10/2020
uid: grpc/browser
ms.openlocfilehash: 333fc8c4277bbac47042d4904c276e963186914a
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172267"
---
# <a name="use-grpc-in-browser-apps"></a><span data-ttu-id="f9de4-103">Usare gRPC nelle app del browser</span><span class="sxs-lookup"><span data-stu-id="f9de4-103">Use gRPC in browser apps</span></span>

<span data-ttu-id="f9de4-104">Di [James Newton-King](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="f9de4-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f9de4-105">**gRPC-supporto Web in .NET è sperimentale**</span><span class="sxs-lookup"><span data-stu-id="f9de4-105">**gRPC-Web support in .NET is experimental**</span></span>
>
> <span data-ttu-id="f9de4-106">gRPC-Web per .NET è un progetto sperimentale, non un prodotto di cui è stato eseguito il commit.</span><span class="sxs-lookup"><span data-stu-id="f9de4-106">gRPC-Web for .NET is an experimental project, not a committed product.</span></span> <span data-ttu-id="f9de4-107">Si desidera:</span><span class="sxs-lookup"><span data-stu-id="f9de4-107">We want to:</span></span>
>
> * <span data-ttu-id="f9de4-108">Testare l'approccio all'implementazione di gRPC-Web Works.</span><span class="sxs-lookup"><span data-stu-id="f9de4-108">Test that our approach to implementing gRPC-Web works.</span></span>
> * <span data-ttu-id="f9de4-109">Ottenere commenti e suggerimenti su se questo approccio è utile per gli sviluppatori .NET rispetto al metodo tradizionale di configurazione di gRPC-Web tramite un proxy.</span><span class="sxs-lookup"><span data-stu-id="f9de4-109">Get feedback on if this approach is useful to .NET developers compared to the traditional way of setting up gRPC-Web via a proxy.</span></span>
>
> <span data-ttu-id="f9de4-110">Invia commenti e suggerimenti in [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) per assicurarsi di creare qualcosa di cui gli sviluppatori desiderano e siano produttivi.</span><span class="sxs-lookup"><span data-stu-id="f9de4-110">Please leave feedback at [https://github.com/grpc/grpc-dotnet](https://github.com/grpc/grpc-dotnet) to ensure we build something that developers like and are productive with.</span></span>

<span data-ttu-id="f9de4-111">Non è possibile chiamare un servizio HTTP/2 gRPC da un'app basata su browser.</span><span class="sxs-lookup"><span data-stu-id="f9de4-111">It is not possible to call a HTTP/2 gRPC service from a browser-based app.</span></span> <span data-ttu-id="f9de4-112">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) è un protocollo che consente alle app JavaScript e blazer del browser di chiamare i servizi gRPC.</span><span class="sxs-lookup"><span data-stu-id="f9de4-112">[gRPC-Web](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-WEB.md) is a protocol that allows browser JavaScript and Blazor apps to call gRPC services.</span></span> <span data-ttu-id="f9de4-113">Questo articolo illustra come usare gRPC-Web in .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f9de4-113">This article explains how to use gRPC-Web in .NET Core.</span></span>

## <a name="configure-grpc-web-in-aspnet-core"></a><span data-ttu-id="f9de4-114">Configurare gRPC-Web in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f9de4-114">Configure gRPC-Web in ASP.NET Core</span></span>

<span data-ttu-id="f9de4-115">i servizi gRPC ospitati in ASP.NET Core possono essere configurati per supportare gRPC-Web insieme a HTTP/2 gRPC.</span><span class="sxs-lookup"><span data-stu-id="f9de4-115">gRPC services hosted in ASP.NET Core can be configured to support gRPC-Web alongside HTTP/2 gRPC.</span></span> <span data-ttu-id="f9de4-116">gRPC-Web non richiede alcuna modifica ai servizi.</span><span class="sxs-lookup"><span data-stu-id="f9de4-116">gRPC-Web does not require any changes to services.</span></span> <span data-ttu-id="f9de4-117">L'unica modifica è la configurazione di avvio.</span><span class="sxs-lookup"><span data-stu-id="f9de4-117">The only modification is startup configuration.</span></span>

<span data-ttu-id="f9de4-118">Per abilitare gRPC-Web con un servizio gRPC ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="f9de4-118">To enable gRPC-Web with an ASP.NET Core gRPC service:</span></span>

* <span data-ttu-id="f9de4-119">Aggiungere un riferimento al pacchetto [Grpc. AspNetCore. Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) .</span><span class="sxs-lookup"><span data-stu-id="f9de4-119">Add a reference to the [Grpc.AspNetCore.Web](https://www.nuget.org/packages/Grpc.AspNetCore.Web) package.</span></span>
* <span data-ttu-id="f9de4-120">Configurare l'app per l'uso di gRPC-Web aggiungendo `AddGrpcWeb` e `UseGrpcWeb` a *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="f9de4-120">Configure the app to use gRPC-Web by adding `AddGrpcWeb` and `UseGrpcWeb` to *Startup.cs*:</span></span>

[!code-csharp[](~/grpc/browser/sample/Startup.cs?name=snippet_1&highlight=10,14)]

<span data-ttu-id="f9de4-121">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="f9de4-121">The preceding code:</span></span>

* <span data-ttu-id="f9de4-122">Aggiunge il middleware gRPC-Web, `UseGrpcWeb`, dopo il routing e prima degli endpoint.</span><span class="sxs-lookup"><span data-stu-id="f9de4-122">Adds the gRPC-Web middleware, `UseGrpcWeb`, after routing and before endpoints.</span></span>
* <span data-ttu-id="f9de4-123">Specifica che il metodo `endpoints.MapGrpcService<GreeterService>()` supporta gRPC-Web con `EnableGrpcWeb`.</span><span class="sxs-lookup"><span data-stu-id="f9de4-123">Specifies the `endpoints.MapGrpcService<GreeterService>()` method supports gRPC-Web with `EnableGrpcWeb`.</span></span> 

<span data-ttu-id="f9de4-124">In alternativa, configurare tutti i servizi per il supporto di gRPC-Web aggiungendo `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` a ConfigureServices.</span><span class="sxs-lookup"><span data-stu-id="f9de4-124">Alternatively, configure all services to support gRPC-Web by adding `services.AddGrpcWeb(o => o.GrpcWebEnabled = true);` to ConfigureServices.</span></span>

[!code-csharp[](~/grpc/browser/sample/AllServicesSupportExample_Startup.cs?name=snippet_1&highlight=6,13)]

<span data-ttu-id="f9de4-125">Potrebbe essere necessaria una configurazione aggiuntiva per chiamare gRPC-Web dal browser, ad esempio la configurazione di ASP.NET Core per il supporto di CORS.</span><span class="sxs-lookup"><span data-stu-id="f9de4-125">Some additional configuration may be required to call gRPC-Web from the browser, such as configuring ASP.NET Core to support CORS.</span></span> <span data-ttu-id="f9de4-126">Per ulteriori informazioni, vedere [supporto di CORS](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="f9de4-126">For more information, see [support CORS](xref:security/cors).</span></span>

## <a name="call-grpc-web-from-the-browser"></a><span data-ttu-id="f9de4-127">Chiamare gRPC-Web dal browser</span><span class="sxs-lookup"><span data-stu-id="f9de4-127">Call gRPC-Web from the browser</span></span>

<span data-ttu-id="f9de4-128">Le app browser possono usare gRPC-Web per chiamare i servizi gRPC.</span><span class="sxs-lookup"><span data-stu-id="f9de4-128">Browser apps can use gRPC-Web to call gRPC services.</span></span> <span data-ttu-id="f9de4-129">Esistono alcuni requisiti e limitazioni quando si chiamano i servizi gRPC con gRPC-Web dal browser:</span><span class="sxs-lookup"><span data-stu-id="f9de4-129">There are some requirements and limitations when calling gRPC services with gRPC-Web from the browser:</span></span>

* <span data-ttu-id="f9de4-130">Il server deve essere stato configurato per supportare gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="f9de4-130">The server must have been configured to support gRPC-Web.</span></span>
* <span data-ttu-id="f9de4-131">Il flusso client e le chiamate di streaming bidirezionali non sono supportate.</span><span class="sxs-lookup"><span data-stu-id="f9de4-131">Client streaming and bidirectional streaming calls aren't supported.</span></span> <span data-ttu-id="f9de4-132">Il flusso del server è supportato.</span><span class="sxs-lookup"><span data-stu-id="f9de4-132">Server streaming is supported.</span></span>
* <span data-ttu-id="f9de4-133">Per chiamare i servizi gRPC in un dominio diverso è necessario configurare [CORS](xref:security/cors) nel server.</span><span class="sxs-lookup"><span data-stu-id="f9de4-133">Calling gRPC services on a different domain requires [CORS](xref:security/cors) to be configured on the server.</span></span>

### <a name="javascript-grpc-web-client"></a><span data-ttu-id="f9de4-134">JavaScript gRPC-client Web</span><span class="sxs-lookup"><span data-stu-id="f9de4-134">JavaScript gRPC-Web client</span></span>

<span data-ttu-id="f9de4-135">È disponibile un client JavaScript gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="f9de4-135">There is a JavaScript gRPC-Web client.</span></span> <span data-ttu-id="f9de4-136">Per istruzioni su come usare gRPC-Web da JavaScript, vedere [scrivere codice client JavaScript con gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span><span class="sxs-lookup"><span data-stu-id="f9de4-136">For instructions on how to use gRPC-Web from JavaScript, see [write JavaScript client code with gRPC-Web](https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld#write-client-code).</span></span>

### <a name="configure-grpc-web-with-the-net-grpc-client"></a><span data-ttu-id="f9de4-137">Configurare gRPC-Web con il client gRPC .NET</span><span class="sxs-lookup"><span data-stu-id="f9de4-137">Configure gRPC-Web with the .NET gRPC client</span></span>

<span data-ttu-id="f9de4-138">Il client gRPC .NET può essere configurato per effettuare chiamate gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="f9de4-138">The .NET gRPC client can be configured to make gRPC-Web calls.</span></span> <span data-ttu-id="f9de4-139">Questa operazione è utile per le app [Webassembly Blazer](xref:blazor/index#blazor-webassembly) , che sono ospitate nel browser e hanno le stesse limitazioni http del codice JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f9de4-139">This is useful for [Blazor WebAssembly](xref:blazor/index#blazor-webassembly) apps, which are hosted in the browser and have the same HTTP limitations of JavaScript code.</span></span> <span data-ttu-id="f9de4-140">La chiamata di gRPC-Web con un client .NET equivale a [http/2 gRPC](xref:grpc/client).</span><span class="sxs-lookup"><span data-stu-id="f9de4-140">Calling gRPC-Web with a .NET client is [the same as HTTP/2 gRPC](xref:grpc/client).</span></span> <span data-ttu-id="f9de4-141">L'unica modifica è la modalità di creazione del canale.</span><span class="sxs-lookup"><span data-stu-id="f9de4-141">The only modification is how the channel is created.</span></span>

<span data-ttu-id="f9de4-142">Per usare gRPC-Web:</span><span class="sxs-lookup"><span data-stu-id="f9de4-142">To use gRPC-Web:</span></span>

* <span data-ttu-id="f9de4-143">Aggiungere un riferimento al pacchetto [Grpc .NET. client. Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) .</span><span class="sxs-lookup"><span data-stu-id="f9de4-143">Add a reference to the [Grpc.Net.Client.Web](https://www.nuget.org/packages/Grpc.Net.Client.Web) package.</span></span>
* <span data-ttu-id="f9de4-144">Verificare che il riferimento a [Grpc .NET. client](https://www.nuget.org/packages/Grpc.Net.Client) Package sia 2.27.0 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="f9de4-144">Ensure the reference to [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) package is 2.27.0 or greater.</span></span>
* <span data-ttu-id="f9de4-145">Configurare il canale per l'uso del `GrpcWebHandler`:</span><span class="sxs-lookup"><span data-stu-id="f9de4-145">Configure the channel to use the `GrpcWebHandler`:</span></span>

[!code-csharp[](~/grpc/browser/sample/Handler.cs?name=snippet_1)]

<span data-ttu-id="f9de4-146">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="f9de4-146">The preceding code:</span></span>

* <span data-ttu-id="f9de4-147">Configura un canale per l'uso di gRPC-Web.</span><span class="sxs-lookup"><span data-stu-id="f9de4-147">Configures a channel to use gRPC-Web.</span></span>
* <span data-ttu-id="f9de4-148">Crea un client ed effettua una chiamata utilizzando il canale.</span><span class="sxs-lookup"><span data-stu-id="f9de4-148">Creates a client and makes a call using the channel.</span></span>

<span data-ttu-id="f9de4-149">Al momento della creazione, nel `GrpcWebHandler` sono disponibili le opzioni di configurazione seguenti:</span><span class="sxs-lookup"><span data-stu-id="f9de4-149">The `GrpcWebHandler` has the following configuration options when created:</span></span>

* <span data-ttu-id="f9de4-150">**InnerHandler**: <xref:System.Net.Http.HttpMessageHandler> sottostante che esegue la richiesta http gRPC, ad esempio `HttpClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="f9de4-150">**InnerHandler**: The underlying <xref:System.Net.Http.HttpMessageHandler> that makes the gRPC HTTP request, for example, `HttpClientHandler`.</span></span>
* <span data-ttu-id="f9de4-151">**Mode**: tipo di enumerazione che specifica se la richiesta di richiesta HTTP gRPC `Content-Type` è `application/grpc-web` o `application/grpc-web-text`.</span><span class="sxs-lookup"><span data-stu-id="f9de4-151">**Mode**: An enumeration type that specifies whether the gRPC HTTP request request `Content-Type` is `application/grpc-web` or `application/grpc-web-text`.</span></span>
    * <span data-ttu-id="f9de4-152">`GrpcWebMode.GrpcWeb` configura il contenuto da inviare senza codifica.</span><span class="sxs-lookup"><span data-stu-id="f9de4-152">`GrpcWebMode.GrpcWeb` configures content to be sent without encoding.</span></span> <span data-ttu-id="f9de4-153">Valore predefinito.</span><span class="sxs-lookup"><span data-stu-id="f9de4-153">Default value.</span></span>
    * <span data-ttu-id="f9de4-154">`GrpcWebMode.GrpcWebText` configura il contenuto con codifica Base64.</span><span class="sxs-lookup"><span data-stu-id="f9de4-154">`GrpcWebMode.GrpcWebText` configures content to be base64 encoded.</span></span> <span data-ttu-id="f9de4-155">Obbligatorio per le chiamate di streaming del server nei browser.</span><span class="sxs-lookup"><span data-stu-id="f9de4-155">Required for server streaming calls in browsers.</span></span>
* <span data-ttu-id="f9de4-156">**HttpVersion**: il protocollo http `Version` usato per impostare [HttpRequestMessage. Version](xref:System.Net.Http.HttpRequestMessage.Version) sulla richiesta http gRPC sottostante.</span><span class="sxs-lookup"><span data-stu-id="f9de4-156">**HttpVersion**: HTTP protocol `Version` used to set [HttpRequestMessage.Version](xref:System.Net.Http.HttpRequestMessage.Version) on the underlying gRPC HTTP request.</span></span> <span data-ttu-id="f9de4-157">gRPC-Web non richiede una versione specifica e non esegue l'override dell'impostazione predefinita, a meno che non sia specificato.</span><span class="sxs-lookup"><span data-stu-id="f9de4-157">gRPC-Web doesn't require a specific version and doesn't override the default unless specified.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f9de4-158">I client gRPC generati hanno metodi Sync e Async per la chiamata di metodi unaria.</span><span class="sxs-lookup"><span data-stu-id="f9de4-158">Generated gRPC clients have sync and async methods for calling unary methods.</span></span> <span data-ttu-id="f9de4-159">Ad esempio, `SayHello` è Sync e `SayHelloAsync` è Async.</span><span class="sxs-lookup"><span data-stu-id="f9de4-159">For example, `SayHello` is sync and `SayHelloAsync` is async.</span></span> <span data-ttu-id="f9de4-160">La chiamata di un metodo di sincronizzazione in un'app webassembly Blaze provocherà la mancata risposta dell'app.</span><span class="sxs-lookup"><span data-stu-id="f9de4-160">Calling a sync method in a Blazor WebAssembly app will cause the app to become unresponsive.</span></span> <span data-ttu-id="f9de4-161">I metodi asincroni devono essere sempre usati in un webassembly blazer.</span><span class="sxs-lookup"><span data-stu-id="f9de4-161">Async methods must always be used in Blazor WebAssembly.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f9de4-162">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="f9de4-162">Additional resources</span></span>

* [<span data-ttu-id="f9de4-163">progetto GitHub di gRPC per client Web</span><span class="sxs-lookup"><span data-stu-id="f9de4-163">gRPC for Web Clients GitHub project</span></span>](https://github.com/grpc/grpc-web)
* <xref:security/cors>
